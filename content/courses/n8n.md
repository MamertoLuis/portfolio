---
title: 'N8N'
date: '2026-10-02'
draft: false
---

### Section 1: Introduction & Fundamentals

- **Course Overview & Setup Options**    
     
    - Course structure, workflow design, and downloadable JSON/HTML resources     
          
        
    - Overview of low-code visual editing and automation boundaries        
          
        
- **n8n Installation Methods**    
      
    - **VPS Deployment:** Setting up Docker, Nginx, Certbot, DNS, and HTTPS access        
          
        
    - **Local Installation:** Verification of Node/npm, global npm setup, and initial `localhost:5678` configuration        
          
        
    - **Cloud & 3rd-Party Hosting:** Overview of managed setups, execution limits, and trial access        
          
        
- **n8n Core Interface & LLMs**    
      
    - Navigation, basic prompt management, and JSON variable handling        
          
        
    - Integrating cloud and local LLMs (OpenAI, Ollama, Claude, Grok, Gemini)       
          
        

### Section 2: Building Block Workflows & Core Concepts

- **Trigger Nodes & Credentials**    
      
    - Execution options: Manual, Schedule, Webhook, Form Submission, and Chat triggers        
          
        
    - OAuth and secure authentication (e.g., Google Sheets API setup)       
          
        
    - **Hands-on Project:** Form Submission to Google Sheets
        
          
        
- **Documentation & AI Agents**    
      
    - Adding rich-text workflow notes for code maintainability        
          
        
    - Configuring AI Agent nodes with memory and attached LLM credentials        
          
        
    - **Hands-on Projects:**        
          
        - Simple Chatbot            
              
            
        - Chatbot with Webhook Website Integration            
              
            
        - Tool-Assisted Chatbot (Calculator, Date/Time, SerpAPI)
            
              
            
- **Retrieval Augmented Generation (RAG)**    
      
    - Document chunking, vector conversion, and vector store retrieval        
          
        
    - **Hands-on Project:** Restaurant menu RAG Chatbot using Supabase & OpenAI Embeddings
        
          
        
- **Error Handling Strategies**    
      
    - **Node-Level:** Retry logic, error outputs, and fallback execution        
          
        
    - **Workflow-Level:** Dedicated error-handling workflows and alert webhooks        
          
        

### Section 3: Social Media & Messaging Automation Projects

- **Messaging Platforms**    
      
    - **Telegram Chatbot:** Telegram trigger integration with OpenAI and SerpAPI search        
          
        
    - **Discord Bots:** Standard channel messaging via OAuth2 and Slash Commands via Cloudflare Workers        
          
        
    - **Slack Channel Bot:** Slash command handling with custom scopes        
          
        
- **Content Publishing & Scraping**
    
      
    - **WordPress Bot:** Topic extraction via Telegram, web research, AI drafting, and automated posting
        
          
        
    - **Reddit Automations:** Fetching subreddit posts, comment filtering, AI comment generation, and batch sentiment analysis
        
          
        
    - **X (Twitter) Poster Bot:** Pulling Google RSS feeds, generating short-form posts with hashtags, and auto-publishing
        
          
        
    - **LinkedIn Bot:** RSS topic ingestion, long-form AI content creation, and publishing
        
          
        
- **Data Transformation & Visualization**
    
      
    - Processing Google Sheets data via code nodes and rendering charts using QuickChart.io
        
          
        

### Section 4: Human-in-the-Loop (HITL) Approval Systems

- **HITL Patterns & Execution Control**
    
      
    - Pausing automation pipelines at key stages for manual verification or intervention
        
          
        
- **Implementation Projects:**
    
      
    - **Gmail HITL:** Converting HTML emails, summarizing, and routing approvals to send or edit drafts
        
          
        
    - **Telegram HITL:** AI text classification and interactive approval/modification loops for tweets
        
          
        
    - **Generic URL HITL:** URL-based decision branching using Wait and Merge nodes for refund approvals
        
          
        

### Section 5: Advanced Automation & Specialized Projects

- **Web Scraping:** LinkedIn profile/job search scraper saving structured URLs to Google Sheets
    
      
    
- **Market Intelligence:** RSS-driven Crypto Sentiment Analysis bot filtered by user queries
    
      
    
- **Media & Asset Generation:**
    
      
    - **WordPress Media Integration:** Automated prompt expansion, PNG generation via Stability AI, media attachment, and Telegram notifications
        
          
        
    - **Template-Based Meme Generator:** Automated template selection and text placement via image APIs
        
          
        
    - **Custom Meme Generator:** AI text placement, image generation via Stability AI, and custom prompt refinement
