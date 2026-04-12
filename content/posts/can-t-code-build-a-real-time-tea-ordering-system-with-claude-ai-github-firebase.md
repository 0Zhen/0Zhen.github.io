---
title: "🧋 Can’t Code? Build a Real-time Tea Ordering System with Claude AI + GitHub + Firebase"
date: 2025-06-10T01:49:58Z
description: "How AI Can Help You Quickly Build a Multi-user, Real-time Synchronized Ordering Website"
canonicalURL: "https://medium.com/@chrislee8613/cant-code-build-a-real-time-tea-ordering-system-with-claude-ai-github-firebase-ae24faf8dc59"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*bRwzEClOYIq2SHNN973HiQ.png"
  relative: false
tags: ["Firebase", "Claude AI", "GitHub", "Web App"]
---
## How AI Can Help You Quickly Build a Multi-user, Real-time Synchronized Ordering Website

## Project Background: Solving Real Pain Points

We’ve all been there: the office wants to order drinks, someone posts the menu in the group chat, and everyone starts adding their orders in sequence. But then…

- Orders get accidentally deleted by others
- People forget to specify sweetness and ice levels
- You know what you want but have to check the menu for prices
- Someone beats you to placing their order while you’re typing
- No one knows the total number of drinks — someone has to count manually

Even when everyone follows the format properly, you still need to manually organize and calculate costs. Sound familiar? As an engineer, I decided to use technology to solve this daily problem and reduce drink-less days.

Since I don’t know web development, I used Claude for development. It took only 3 hours from concept to deployment.

## AI-Assisted Development: From Idea to Implementation

## First Conversation: Requirements Analysis and Architecture Suggestions

**My requirements:**

> *“I want a website where everyone can order drinks with various options like sweetness and ice levels, everyone can see what others ordered, and finally calculate the total amount.”*

**Claude’s Technical Suggestions:**

Claude proactively analyzed my needs and suggested the most suitable tech stack:

- GitHub Pages (free website hosting)
- Firebase Firestore (real-time database)
- Pure frontend architecture (simplified deployment)

## Second Conversation: Cost Considerations and Security Protection

**My concerns:**

> *“Will this cost money? What are the cost risks?”*

**Claude’s solutions:**

1. Detailed free tier analysis: calculating actual usage vs free limits
2. Cost control suggestions: password protection to limit users, usage monitoring setup
3. Multi-layer protection: Firebase security rules + frontend restrictions

## Third Conversation: Feature Optimization

**Continuous requirement iteration:**

> *“Can we add sweetness and ice options? And pricing calculations for add-ons?”*

**Claude immediately:**

- Extended menu data structure
- Implemented dynamic price calculations
- Optimized user experience flow

## Key Advantage of AI Collaboration: Contextual Understanding

Throughout the development process, Claude consistently remembered:

- Technical choices and their rationale
- Previously discussed cost considerations
- Background of each feature iteration
- User experience priority considerations

## Engineer’s Guide to AI Collaboration

## 💡 Efficient AI Collaboration Tips

**✅ Clear requirement descriptions:**

❌ “Help me make a website”
 ✅ “I want to create a drink ordering webpage with menu selection, customization options, and real-time multi-user collaboration”

❌ “Add a feature”
 ✅ “I want to add password protection so only people with the password can use the system”

❌ “There’s a problem”
 ✅ “I followed the steps but got a ‘permission denied’ error. Is this a Firebase rules configuration issue?”

## 🎯 Leveraging AI Value in Development:

1. **Rapid prototyping**: Quick implementation from idea to MVP
2. **Technology selection advice**: Recommending suitable tech stacks based on requirements
3. **Code generation**: Reducing repetitive boilerplate code
4. **Problem diagnosis**: Quickly identifying and solving development issues
5. **Best practice guidance**: Security, performance, and maintainability recommendations

## 🔄 Agile Iterative Development Process

- **Version 1**: Basic functionality → AI helps generate MVP
- **Version 2**: Security protection → AI designs passwords and rules
- **Version 3**: Experience optimization → AI improves UI/UX
- **Version 4**: Feature expansion → AI implements new requirements

**Core advantage: Rapid iteration, immediate feedback, continuous improvement!**

## Technical Architecture: AI’s Perfect Combination

## GitHub Pages: Free Static Website Hosting

**AI recommendation reasons:**

- ✅ Completely free: No costs required
- ✅ Automatic deployment: Push code for automatic updates
- ✅ HTTPS support: Built-in SSL certificates
- ✅ Beginner-friendly: Web interface operation, no command line needed

## Firebase Firestore: Real-time Database

**AI recommendation reasons:**

- ✅ Real-time sync: Multi-user collaboration without conflicts
- ✅ Generous free tier: More than enough for daily use
- ✅ NoSQL flexibility: No need to predefine table structures
- ✅ Built-in security rules: AI can help design rules

## Pure Frontend Architecture: Simplified Deployment

**AI recommendation reasons:**

- ✅ No backend needed: Reduces architecture complexity
- ✅ Easy maintenance: Single HTML file
- ✅ Rapid iteration: Changes take effect immediately
- ✅ AI-friendly: Suitable for AI generation and modification

## Core Feature Implementation: AI-Assisted Complete Solutions

## 🔐 Password Protection System

```javascript
// AI-suggested security protection solution
const SYSTEM_PASSWORD = 'tea2025';
function checkPassword() {
  if (inputPassword === SYSTEM_PASSWORD) {
    localStorage.setItem('teaOrderAuth', 'true');
    showMainPage();
  }
}
```

**Design background:** Based on cost control considerations, Claude suggested adding password protection:

- Limit user scope to control Firebase usage
- LocalStorage remembers login state for better experience
- Password can be changed and redeployed anytime
- Provided multiple security levels to choose from

## 🧋 Complete Ordering Process

```cpp
const menuData = {
  "Original Tea House": {
    "Night Rhythm Black Tea": 35,
    "Jin Xuan Honey Black Tea": 35,
    // ... more items
  },
  // ... more categories
};
```

**Feature highlights:**

- Categorized menu structure for easy management
- Customizable sweetness, ice, and add-on options
- Automatic price calculation (base price + add-on fees)
- Real-time form validation

## ⚡ Real-time Data Synchronization

```javascript
// Firebase real-time listening
const q = query(ordersRef, orderBy('timestamp', 'desc'));
unsubscribe = onSnapshot(q, (querySnapshot) => {
  const orders = [];
  querySnapshot.forEach((doc) => {
    orders.push({ id: doc.id, ...doc.data() });
  });
  updateOrderTable(orders);
});
```

**Technical highlights:**

- Using `onSnapshot` for real-time updates
- When anyone adds an order, everyone sees it immediately
- Automatic handling of network disconnection and reconnection

## Firebase Security Rules: Balancing Security and Usability

```csharp
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /orders/{orderId} {
      // Business hours restriction
      allow read: if isWithinBusinessHours();
      
      // Data validation
      allow create: if isWithinBusinessHours() && 
        isValidOrder(request.resource.data);
      
      allow delete: if isWithinBusinessHours();
      allow update: if false; // No modifications allowed
    }
    
    function isWithinBusinessHours() {
      let hour = request.time.hours() + 8; // UTC+8
      return hour >= 9 && hour <= 22;
    }
    
    function isValidOrder(data) {
      return data.customerName is string &&
        data.quantity <= 10 &&
        data.subtotal <= 500;
    }
  }
}
```

**Security considerations:**

- Business hours restriction (9:00–22:00)
- Data format validation
- Quantity and amount reasonability checks
- Prohibition of modifying submitted orders

## Deployment Process: Step-by-Step with AI

## Step 1: Create GitHub Repository

**My confusion:** “What is GitHub? How do I use it?”

**Claude’s guidance:**

1. Go to GitHub.com and register an account
2. Click “New repository”
3. Name it: tea-order-system
4. Set as Public (important!)
5. Click “Create repository”

**Result:** Completed in 5 minutes, simpler than expected!

## Step 2: Upload Code

**My confusion:** “How do I upload code?”

**Claude’s guidance:**

1. Copy the complete AI-generated code
2. Paste into Notepad, save as index.html
3. Click “uploading an existing file” on GitHub
4. Drag and drop the file to upload

**Result:** Completed in 3 minutes, no command line needed!

## Step 3: Enable GitHub Pages

**My confusion:** “How do I make the website live?”

**Claude’s guidance:**

- Settings → Pages → Source: Deploy from a branch
- Branch: main → Save
- Wait 2–3 minutes for access

**Result:** URL automatically generated `[https://myusername.github.io/tea-order-system](https://myusername.github.io/tea-order-system)`

## Step 4: Firebase Setup

**My confusion:** “Firebase setup looks complicated…”

**Claude’s solution:**

- Provided detailed illustrated tutorials
- Auto-generated Firebase configuration code
- Even wrote security rules for me

**Total time:** 30 minutes for first setup, 1 minute for subsequent updates!

## Real Usage Experience

## User Perspective

1. **Simple login:** Enter password to use
2. **Intuitive operation:** Select category → Select item → Customize → Confirm
3. **Real-time feedback:** See others’ orders appear instantly
4. **Mobile-friendly:** Responsive design, seamless mobile experience

## Administrator Perspective

1. **Real-time monitoring:** Statistics panel shows order count and amount
2. **One-click export:** CSV format for easy processing
3. **Flexible management:** Can delete incorrect orders, clear and restart

## Cost Analysis: Is It Really Completely Free?

## GitHub Pages

- **Cost:** $0
- **Limits:** 100GB traffic/month, 100GB storage
- **Assessment:** More than sufficient for internal use

## Firebase Firestore

- **Free tier:**
- 50,000 reads/day
- 20,000 writes/day
- 1GB storage
- **Actual usage (50-person team):**
- Reads: ~500/day
- Writes: ~50/day
- Storage: ~10KB
- **Assessment:** Completely within free limits

## Challenges Encountered and AI-Assisted Solutions

## Challenge 1: Firebase Permission Error

**Problem description:** “Getting `permission-denied` error"

**Information provided to Claude:** Error screenshot + specific operation steps

**Claude’s diagnosis and solution:**

> *“This is a Firebase security rules issue, caused by… Suggested solution:*

> Go to Firebase Console → Rules

> Replace with the following rules: [provided specific code]

> Click Publish”

## Challenge 2: Cost Control Requirements

**My concern:** “Firebase usage billing makes me a bit worried”

**Claude’s response:**

- Detailed analysis of free tier vs expected usage
- Designed multi-layer protection mechanisms (password + security rules)
- Provided usage monitoring and budget alert setup methods

**Result:** Complete cost control strategy

## Technical Advantages of AI Collaboration

**Traditional debugging process:** Problem discovery → Google search → Read documentation → Try solution → Test verification

**AI-assisted process:** Describe problem → AI analysis and diagnosis → Provide solution → Direct implementation

**Efficiency improvement:** Problem-solving speed increased 5–10x

## Project Results: Development Efficiency Through AI Collaboration

## Development Time Analysis

**Traditional independent development estimate:**

- Technology selection research: 2–4 hours
- Architecture design: 2–3 hours
- Frontend development: 8–12 hours
- Firebase integration: 3–5 hours
- Security rules design: 2–4 hours
- Testing and debugging: 4–6 hours
- **Total: 21–34 hours**

**AI collaboration actual:**

- Requirements discussion and architecture design: 30 minutes
- AI code generation and adjustments: 1.5 hours
- Deployment and configuration: 1 hour
- Feature testing and optimization: 30 minutes
- **Total: 3 hours**

## AI Development Advice for Non-Technical People

## 💡 Golden Rules for AI Collaboration

**1. Describe requirements in detail**

❌ “Make a website”
 ✅ “I want to create an internal drink ordering system with password protection, real-time sync, mobile-friendly, for about 40 users”

**2. Provide specific usage scenarios**

✅ “Our company orders drinks collectively every Friday. Previously we used LINE group statistics which was troublesome. We want a system where everyone can select directly, and it automatically calculates total amount and quantity”

**3. Don’t be afraid to ask for details**

✅ “You mentioned Firebase, can you explain in detail what it is?”
 ✅ “I don’t quite understand this step, can you be more detailed?”
 ✅ “Is there a simpler method?”

**4. Provide timely problem feedback**

✅ Post error screenshots
 ✅ Describe specific operation steps
 ✅ Explain expected vs actual results

## 🚀 Future Possibilities of AI Development

This experience showed me that AI is lowering technical barriers:

1. **Everyone can be a creator:** Execution and ideas matter more than owning technical skills
2. **Rapid prototype validation:** From idea to product in hours
3. **Continuous iterative improvement:** Propose new requirements anytime, AI implements immediately
4. **Extremely low learning cost:** Learn while doing, no upfront investment needed

## 📈 Project Types Suitable for AI-Assisted Development

**✅ Highly suitable:**

- Internal tools (ordering systems, voting systems)
- Personal projects (budgeting, to-do lists)
- Prototype validation (quickly testing ideas)
- Static websites (portfolios, documentation)

## Conclusion: Engineering Practice of AI Collaboration

## Three Important Insights:

1. **AI accelerated the idea-to-implementation process:** Allows us to validate technical solutions faster
2. **Human-AI collaboration created new development paradigms:** AI handles execution, humans handle decision-making and creativity
3. **Engineer value lies in systems thinking:** Technical implementation became simpler, architectural design became more important

## Impact on Engineering Practice

**Short-term impact:**

- Improved development efficiency, shortened product iteration cycles
- Reduced repetitive work, increased innovation time
- Improved code quality, reduced low-level errors

**Long-term trends:**

- Engineer role shifts from “implementer” to “designer”
- Cross-disciplinary collaboration becomes increasingly important
- Continuous learning and adaptability become core competitive advantages

Most importantly, this project made ordering drinks no longer troublesome. Seeing colleagues no longer find order statistics bothersome reminded me: technology’s value always lies in solving real problems and making life better.

In the AI era, we have more powerful tools, but the essence of engineering hasn’t changed: create value with technology, change the world with code.

![Website Screenshots](https://cdn-images-1.medium.com/max/800/1*bRwzEClOYIq2SHNN973HiQ.png)
*Website Screenshots*

![Firebase usage after going live on 2025/6/5](https://cdn-images-1.medium.com/max/800/0*VFc-CQ2V6KswDpTh.png)