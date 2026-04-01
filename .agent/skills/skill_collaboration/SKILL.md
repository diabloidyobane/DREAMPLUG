# SKILL: REAL-TIME COLLABORATION

**Goal:** Enable real-time collaborative features for plugin development and performance.
**Trigger:** Optional feature during `/plan` or `/impl` phases
**Output Location:** `plugins/[Name]/Source/Collaboration/`

---

## Overview

Real-time collaboration enables multiple users to interact with plugin parameters simultaneously. Use cases:
1. **Collaborative mixing** — Multiple engineers tweaking parameters remotely
2. **Live performance** — Synchronized parameter changes across networked instances
3. **Teaching** — Instructor demonstrates settings that students see in real-time

---

## Architecture

### OSC (Open Sound Control) Based
JUCE has built-in OSC support via `juce_osc` module.

```
┌──────────────┐     OSC Messages     ┌──────────────┐
│  Instance A  │ ◄──────────────────► │  Instance B  │
│  (Host)      │    /param/gain 0.75  │  (Client)    │
└──────────────┘                      └──────────────┘
```

### Parameter Sync Protocol
```
OSC Address Pattern:
/[PluginName]/param/[paramID]  [float value]
/[PluginName]/preset/load      [string presetName]
/[PluginName]/sync/request     []
/[PluginName]/sync/state       [blob xmlState]
```

## STEP 1: OSC Server/Client

### Add to CMakeLists.txt
```cmake
target_link_libraries(${PLUGIN_NAME} PRIVATE juce::juce_osc)
```

### OSC Sender (broadcast parameter changes)
```cpp
#include <juce_osc/juce_osc.h>

class CollaborationManager : public juce::AudioProcessorValueTreeState::Listener
{
public:
    CollaborationManager(juce::AudioProcessorValueTreeState& apvts)
        : apvts(apvts)
    {
        // Listen to all parameter changes
        for (auto* param : apvts.processor.getParameters())
            if (auto* p = dynamic_cast<juce::RangedAudioParameter*>(param))
                apvts.addParameterListener(p->paramID, this);
    }

    void startHosting(int port = 9001)
    {
        sender.connect("255.255.255.255", port); // Broadcast
        isHosting = true;
    }

    void parameterChanged(const juce::String& paramID, float newValue) override
    {
        if (isHosting && !isReceiving)
        {
            juce::OSCMessage msg("/dreamplug/param/" + paramID);
            msg.addFloat32(newValue);
            sender.send(msg);
        }
    }

private:
    juce::AudioProcessorValueTreeState& apvts;
    juce::OSCSender sender;
    bool isHosting = false;
    bool isReceiving = false; // Prevent feedback loops
};
```

### OSC Receiver (apply remote parameter changes)
```cpp
class CollaborationReceiver : private juce::OSCReceiver::Listener<juce::OSCReceiver::MessageLoopCallback>
{
public:
    CollaborationReceiver(juce::AudioProcessorValueTreeState& apvts)
        : apvts(apvts)
    {
        receiver.addListener(this);
    }

    void startListening(int port = 9001)
    {
        receiver.connect(port);
    }

    void oscMessageReceived(const juce::OSCMessage& message) override
    {
        auto address = message.getAddressPattern().toString();
        if (address.startsWith("/dreamplug/param/") && message.size() == 1)
        {
            auto paramID = address.fromLastOccurrenceOf("/", false, false);
            auto value = message[0].getFloat32();

            if (auto* param = apvts.getParameter(paramID))
                param->setValueNotifyingHost(param->convertTo0to1(value));
        }
    }

private:
    juce::AudioProcessorValueTreeState& apvts;
    juce::OSCReceiver receiver;
};
```

## STEP 2: Collaboration UI

### WebView Collaboration Panel
```html
<div class="collab-panel">
    <div class="collab-status">
        <span class="status-dot offline"></span>
        <span>Not Connected</span>
    </div>
    <button class="host-btn">Host Session</button>
    <button class="join-btn">Join Session</button>
    <input class="ip-input" placeholder="IP Address" />
    <div class="connected-users">
        <!-- User avatars / names -->
    </div>
</div>
```

### Visage Collaboration Panel
```cpp
class CollabPanel : public juce::Component
{
    juce::TextButton hostButton { "Host" };
    juce::TextButton joinButton { "Join" };
    juce::TextEditor ipInput;
    juce::Label statusLabel { {}, "Not Connected" };
};
```

## STEP 3: Session Management

```cpp
struct CollabSession {
    juce::String sessionId;    // Unique session identifier
    juce::String hostName;     // Display name of host
    int port;                  // OSC port
    bool isHost;               // Am I the host?
    juce::StringArray peers;   // Connected peer names
};
```

## ⚠️ CRITICAL RULES
1. **Prevent feedback loops** — Flag when receiving vs. sending changes
2. **Rate-limit updates** — Don't flood the network (max ~60 updates/sec per parameter)
3. **Graceful disconnect** — Handle network drops without crashing
4. **Audio thread safety** — OSC callbacks happen on message thread, use `AsyncUpdater` to bridge
5. **Firewall friendly** — Document required port openings
