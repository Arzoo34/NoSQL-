## DESIGN PROCESS

The Design Process is a structured, step-by-step method for creating a user interface starting from understanding the problem and ending with a tested, working design.

### The User-Centered Design (UCD) Process
User-Centered Design means the user is involved at every stage - not just at the end.

## The 5 Stages of the Design Process 

```text
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   1. ANALYSIS  →  2. DESIGN  →  3. PROTOTYPE           │
│        ↑                              ↓                 │
│        │                              │                 │
│   5. IMPROVE  ←  4. EVALUATE  ←───────┘                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

|Stage|What Happens|Example|
|---|---|---|
|**1. Analysis**|Understand users, tasks, and needs|Interview users of a banking app|
|**2. Design**|Create the interface concept|Sketch screens, decide layout|
|**3. Prototype**|Build a working model|Clickable mockup in Figma|
|**4. Evaluate**|Test with real users|Watch 5 users try the app|
|**5. Improve**|Fix problems found|Redesign confusing button|
### Why This Process Matters

|Without Process|With Process|
|---|---|
|Guess what users want|Know what users want|
|Build wrong thing|Build right thing|
|Expensive fixes later|Cheap fixes early|
|Users frustrated|Users satisfied|
### Real-World Example: Designing a Food Delivery App

1. **Analysis:** Talk to users — they want fast ordering, clear prices, live tracking.
    
2. **Design:** Sketch home screen with search bar, restaurant cards, cart icon.
    
3. **Prototype:** Build a clickable demo.
    
4. **Evaluate:** Test with 10 users — they can't find the "Checkout" button.
    
5. **Improve:** Make "Checkout" bigger and move it to the bottom.
    

**Result:** A better app because the process was followed.
### Key Principle: Iteration

**Iteration** means repeating the cycle — design, test, fix, test again — until it's right. You rarely get it perfect the first time.

## Human Interaction with Computers
Human Interaction with Computers is the study of how people and machines exchange information what the human does, what the computers do, and how they communicate.

```text
┌──────────┐                    ┌──────────┐
│  HUMAN   │ ──── Input ────►   │ COMPUTER │
│          │                    │          │
│          │ ◄─── Output ────   │          │
└──────────┘                    └──────────┘
     ↑                               ↑
  Senses                          Processes
  (eyes, ears,                    (CPU, memory,
   touch)                          software)
```
### The Two Sides of Interaction

|Human Side|Computer Side|
|---|---|
|Perceives (sees, hears)|Displays (screen, sound)|
|Thinks (processes)|Computes (CPU)|
|Acts (types, clicks)|Responds (feedback)|
|Remembers (memory)|Stores (RAM, disk)|
### Types of Interaction

|Type|Human Does|Computer Does|Example|
|---|---|---|---|
|**Command**|Types instructions|Executes|`Ctrl+S` to save|
|**Conversational**|Talks naturally|Understands|Siri, Alexa|
|**Manipulative**|Drags, clicks|Responds visually|Dragging files|
|**Navigational**|Moves through space|Shows new view|Scrolling a webpage|
|**Exploratory**|Explores freely|Reveals info|Browsing a map|
### The Interaction Cycle (Norman's Model)

**Don Norman** described interaction as a 7-step cycle:

1. **Goal:** "I want to save my file"
    
2. **Plan:** "I'll press Ctrl+S"
    
3. **Specify:** Fingers move to keys
    
4. **Execute:** Keys pressed
    
5. **Perceive:** Screen shows "Saved"
    
6. **Interpret:** "It worked"
    
7. **Evaluate:** "Goal achieved"
    

**If any step fails → user confusion.**

### Example: Using an ATM

|Step|Human|Computer|
|---|---|---|
|1|Insert card|Reads card|
|2|Enter PIN|Verifies PIN|
|3|Select "Withdraw"|Shows amount options|
|4|Enter amount|Dispenses cash|
|5|Take cash|Prints receipt|

**This is interaction — a back-and-forth conversation.**

### Key Takeaway

Interaction is **not one-way**. The computer must **respond** to every human action, and the human must **understand** the computer's response.

## Importance of Human Characteristics

Human Characteristics are the physical, cognitive and emotional abilities and limitations of people that must be considered when designing interfaces.
### Why This Matters

Computers are **fast, precise, and tireless**.  
Humans are **slow, error-prone, and get tired**.

If we design interfaces that ignore human limits → **frustration, errors, failure**.
### The Three Types of Human Characteristics

|Type|What It Covers|Example|
|---|---|---|
|**Physical**|Body, senses, movement|Eyesight, hand size, reaction time|
|**Cognitive**|Mind, memory, thinking|Attention span, learning speed|
|**Emotional**|Feelings, motivation|Stress, frustration, enjoyment|
### Real-World Example: ATM for Elderly Users

|Ignoring Human Characteristics|Considering Human Characteristics|
|---|---|
|Tiny text|Large, bold text|
|30-second timeout|2-minute timeout with warning|
|Complex menu|Simple 3-option menu|
|No audio|Voice guidance available|
## Human Considerations in Interface Design
Human Considerations are the specific factors designers must think about to make an interface usable for real people.
### The 6 Key Human Considerations

|#|Consideration|Meaning|Example|
|---|---|---|---|
|1|**Perception**|How users see/hear the interface|Color contrast, font size|
|2|**Memory**|How users remember things|Don't force recall|
|3|**Attention**|What users focus on|Highlight important items|
|4|**Learning**|How users learn|Consistent patterns|
|5|**Motivation**|Why users use it|Make it rewarding|
|6|**Error**|How users make mistakes|Prevent and recover|
### Detailed Explanation

**1. Perception**

- Users perceive via sight, sound, touch
    
- Design must match human perceptual limits
    
- _Example:_ Red text on green background is hard to read
    

**2. Memory**

- **Short-term:** ~7 items, ~20 seconds
    
- **Long-term:** Learned patterns
    
- _Example:_ Show recently used files instead of making users remember paths
    

**3. Attention**

- Users can't focus on everything
    
- Use visual hierarchy: size, color, position
    
- _Example:_ Big "Buy Now" button stands out
    

**4. Learning**

- Users learn by doing, not reading manuals
    
- Consistency helps learning
    
- _Example:_ Same "Save" icon everywhere
    

**5. Motivation**

- Users are motivated by goals, rewards, progress
    
- _Example:_ Progress bars show how close you are
    

**6. Error**

- Humans WILL make mistakes
    
- Design to prevent, detect, and recover
    
- _Example:_ "Undo" button, confirmation dialogs
    

### Real-World Example: Online Shopping Checkout

|Consideration|Good Design|
|---|---|
|Perception|Clear price, large "Place Order" button|
|Memory|Saved addresses, no re-typing|
|Attention|Progress bar: "Step 2 of 3"|
|Learning|Same checkout flow on all sites|
|Motivation|"Free shipping on orders over ₹500"|
|Error|"Are you sure?" before payment|

### Key Takeaway

Every design decision should answer: **"Does this help the human?"**

---

## Human Interaction Speeds

### Definition

**Human Interaction Speed** is how fast a person can perceive, think, and respond — compared to how fast a computer can process and respond.

### The Speed Mismatch

||Human|Computer|
|---|---|---|
|**Processing**|~50-100 ms per thought|~1 nanosecond|
|**Response**|~250 ms reaction|~1 millisecond|
|**Typing**|~40 words/min|~1 billion ops/sec|
|**Reading**|~200-300 words/min|Instant|
|**Memory recall**|Seconds|Instant|

**Computers are millions of times faster — but humans are in control.**

### Key Human Speed Limits

|Action|Typical Speed|Design Implication|
|---|---|---|
|**Reaction time**|250 ms|Don't require instant clicks|
|**Typing**|40 WPM|Provide shortcuts, autocomplete|
|**Reading**|250 WPM|Keep text concise|
|**Decision making**|1-3 seconds|Don't rush users|
|**Learning new task**|Minutes to hours|Provide guidance|

### The 3 Response Time Limits (Nielsen)

|Time|User Perception|Design Rule|
|---|---|---|
|**< 0.1 sec**|Instant|No feedback needed|
|**0.1 – 1 sec**|Slight delay|Show busy cursor|
|**1 – 10 sec**|Noticeable wait|Show progress bar|
|**> 10 sec**|Too long|Show progress + allow cancel|

### Real-World Example: Website Loading

|Load Time|User Reaction|
|---|---|
|0.5 sec|"Fast!"|
|2 sec|"Okay"|
|5 sec|"Slow..."|
|10+ sec|"I'm leaving"|

**Amazon found:** Every 100ms delay = 1% sales loss.

### Why This Matters

- **Don't make users wait unnecessarily.**
    
- **Don't rush users either.**
    
- **Match system speed to human speed.**
    
- **Provide feedback during delays.**
    

### Key Takeaway

Design for **human speed**, not computer speed. A fast system that confuses humans is worse than a slower system that's clear.

---

## Understanding Business Functions

### Definition

**Business Functions** are the goals, processes, and requirements of the organization that the interface is being built for.

### Why Designers Must Understand Business

An interface isn't just a pretty screen — it must **help the business achieve its goals**.

|Business Goal|Interface Implication|
|---|---|
|Increase sales|Easy checkout, recommendations|
|Reduce support calls|Clear help, intuitive design|
|Improve productivity|Fast workflows, shortcuts|
|Build brand loyalty|Consistent, pleasant experience|
|Reduce costs|Automation, self-service|

### Key Business Considerations

|Consideration|Meaning|Example|
|---|---|---|
|**Business model**|How the company makes money|Subscription vs. ads|
|**Target users**|Who uses the system|Customers, employees, partners|
|**Workflow**|How work gets done|Order → Ship → Invoice|
|**Constraints**|Budget, time, tech limits|Legacy systems|
|**Competition**|What rivals offer|Feature parity|
|**Regulations**|Legal requirements|GDPR, accessibility laws|

### Real-World Example: Banking App

|Business Function|Interface Design|
|---|---|
|Increase deposits|Prominent "Deposit" button|
|Reduce branch visits|Mobile check deposit|
|Cross-sell products|"You might also like..."|
|Meet regulations|Clear terms, audit trails|
|Build trust|Security badges, encryption info|

### The Designer's Role

A good designer:

- **Talks to stakeholders** (managers, employees, customers)
    
- **Understands the workflow** (how work actually happens)
    
- **Balances user needs with business needs**
    
- **Asks:** "Does this help the business AND the user?"
    

### Key Takeaway

**Great interfaces serve both users and business.** If it only serves one, it fails.