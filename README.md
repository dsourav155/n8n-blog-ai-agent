**n8n Workflow Documentation:\
Automated AI-Powered Blog Creation for Webflow**
================================================

## **Overview**

This n8n workflow automates creating and publishing SEO-optimized blog posts on Webflow, leveraging AI models (via OpenRouter and Deepseek AI) to generate engaging titles and content. Designed for business owners, AI automation agencies, and DevOps professionals, the workflow streamlines content creation by allowing users to input a topic and automatically produce high-quality, search-engine-optimized blog posts (1,000–1,500 words) focused on AI and DevOps technologies. The posts are then published to a Webflow blog template, positioning your company or project (e.g., Knnekt) as a thought leader in the AI and DevOps space.

***


## **Workflow Purpose**

- **Simplify Content Creation**: Enable users to generate professional blog posts with minimal effort by entering a single topic.

-  **Enhance SEO Visibility**: Produce titles and articles optimized for search engines to drive traffic to your Webflow site.

- **Target Audience**: Business owners integrating AI and DevOps, AI automation agency leaders, and technical professionals seeking innovative solutions.

- **Alignment with Expertise**: Leverages my expertise in DevOps, AI, and automation (e.g., CI/CD, Kubernetes, AI tools) to create a scalable, efficient workflow.

***


## **Workflow Components**

The workflow consists of six interconnected nodes, each performing a specific function. Below is a breakdown of the nodes and their roles, based on the provided JSON document:


### **1. When Chat Message Received (@n8n/n8n-nodes-langchain.chatTrigger)**

- **Function**: Acts as the entry point, receiving the topic input (e.g., “Why should we use AI”) via a chat trigger or webhook.

-  **Position**: \[0, 0]

-  **Webhook ID**: a1ecd8b1-2161-4b8a-b4ac-c4ea99e4e672

-  **Output**: Passes the topic to the next node for title generation.


### **2. Title Agent (@n8n/n8n-nodes-langchain.agent)**

- **Function**: Uses the Deepseek AI model (via OpenRouter) to generate a concise, SEO-optimized blog title (max 8 words, no quotations, single title only).

- **System Message**: “You are a writer that writes high-ranking SEO blog titles. The title should exceed no more than 8 words. DO NOT PUT QUOTATION AROUND THE TITLE. Create only 1 title.”

- **Position**: \[220, 0]

- **Input**: Topic from “When Chat Message Received.”

- **Output**: A single blog title (e.g., “Boost Business with AI Efficiency”).


### **3. OpenRouter Chat Model (@n8n/n8n-nodes-langchain.lmChatOpenRouter)**

- **Function**: Powers the Title Agent by connecting to the Deepseek AI model (deepseek-r1-distill-llama-70b:free) through the OpenRouter API.

- **Position**: \[260, 220]

- **Credentials**: OpenRouter API (ID: your-api-key, Name: “OpenRouter account”)

- **Output**: AI-generated title passed back to the Title Agent.


### **4. Blog Body Agent (@n8n/n8n-nodes-langchain.agent)**

- **Function**: Generates a detailed, SEO-optimized blog post (1,000–1,500 words) based on the title generated in Step 2. The content focuses on AI and DevOps technologies, including automation, AI chatbots, and their impact on businesses.

- **System Message**: “As a professional journalist specializing in artificial intelligence and DevOps, you are tasked with creating blog posts centered on AI and DevOps technologies, including automation, AI chatbots, and their impact on businesses. Your articles should explore how businesses can leverage AI and DevOps for efficiency, with a particular focus on how AI automation can enhance operational efficiency. Each post must be unique, with at least 1,000 to 1,500 words, and should be optimized for search engines to boost visibility for our company website. It should cater to business owners interested in integrating AI and DevOps into their operations and also appeal to AI automation agency owners by providing strategic insights into expanding their businesses through AI and DevOps innovation. The ultimate objective is to make our company a go-to resource for business owners and agency leaders looking to capitalize on AI and DevOps technologies. Your writing should be informative and engaging, with examples and real-world scenarios, designed to drive traffic to our site and position our company as a thought leader in the AI and DevOps space. IMPORTANT: ONLY USE HTML LANGUAGE TO CREATE THE BLOG. DO NOT IMPORT OR INSERT THE IMAGES. DO NOT WRITE ANYTHING EXCEPT FOR THE BLOG.”

- **Position**: \[580, 0]

- **Input**: Title from the Title Agent.

- **Output**: HTML-formatted blog post (e.g., \<h1>Boost Business with AI Efficiency\</h1>\<p>Introduction to AI and DevOps...\</p>).


### **5. OpenRouter Chat Model1 (@n8n/n8n-nodes-langchain.lmChatOpenRouter)**

- **Function**: Powers the Blog Body Agent by connecting to the Deepseek AI model (deepseek-r1-distill-llama-70b:free) through the OpenRouter API.

- **Position**: \[620, 220]

- **Credentials**: OpenRouter API (ID:your-api-key, Name: “OpenRouter account”)

- **Output**: AI-generated HTML blog post passed back to the Blog Body Agent.


### **6. Webflow (n8n-nodes-base.webflow)**

- **Function**: Publishes the generated title and blog post to a Webflow blog collection, automatically creating a new blog post on your chosen Webflow site.

- **Operation**: Create

- **Fields**:

  - name: Dynamically set to the title from the Title Agent (={{ $('Title Agent').item.json.output }}).

  - post-body: Dynamically set to the HTML blog post from the Blog Body Agent (={{ $json.output }}).

-

- **Position**: \[940, 0]

- **Credentials**: Webflow OAuth2 API (ID: abc, Name: “Webflow account”)

- **Output**: Blog post published on Webflow, accessible via your site’s blog template.

***


## **Workflow Flow**

1. A user inputs a topic (e.g., “Why should we use AI”) via the chat trigger/webhook.

2. The Title Agent uses Deepseek AI (via OpenRouter) to generate a concise, SEO-optimized title (max 8 words).

3. The Blog Body Agent uses Deepseek AI to create a detailed, HTML-formatted blog post (1,000–1,500 words) based on the title, focusing on AI and DevOps.

4. The Webflow node publishes the title and blog post to your Webflow blog collection, creating a new blog post automatically.

***



## **Prerequisites**

To use this workflow, you’ll need:

- **n8n Account**: Set up and running (self-hosted or cloud-based).

- **OpenRouter API Key**: Obtain an API key for OpenRouter and configure it as “OpenRouter account” (ID:your-api-key) in n8n. Ensure access to the Deepseek AI model (deepseek-r1-distill-llama-70b:free).

- **Webflow Account**: Create a Webflow site with a blog collection and obtain OAuth2 credentials (ID: example, Name: “Webflow account”). Configure the site ID (abc) and collection ID (xyz) in the Webflow node.

- **Basic Knowledge**: Familiarity with n8n, APIs, and Webflow is recommended but not required, as the workflow is designed for ease of use.

***


## **Setup Instructions**

1. **Install n8n**: If not already installed, set up n8n on your preferred platform (cloud or self-hosted).

2. **Import Workflow**: Use the provided JSON document to import the workflow into n8n.

3. **Configure Credentials**:

   - In n8n, navigate to **Credentials** and add:

     - An OpenRouter API credential named “OpenRouter account” with your API key (ID: your-api-key).

     - A Webflow OAuth2 credential named “Webflow account” with your Webflow API access (ID: abc).


4. **Set Up Webflow**:

   - Ensure your Webflow site has a blog collection with fields for “name” (title) and “post-body” (HTML content).

   - =Update the siteId and collectionId in the Webflow node to match your Webflow setup if they differ from the provided values.


5. **Test the Workflow**:

   - Trigger the workflow by sending a chat message or webhook request with a topic (e.g., “Why should we use AI”).

   - Verify the output: Check the generated title, blog post (in HTML), and the published Webflow blog post.

***


## **Use Cases**

- **Startup Content Strategy**: Automate blog creation for your startup (e.g., Knnekt) to drive traffic and establish thought leadership in AI and DevOps.

- **Agency Growth**: AI automation agencies can use this to generate client content, showcasing strategies for integrating AI and DevOps.

- **Personal Branding**: Leverage this workflow to create content for your personal brand, reinforcing your expertise in DevOps, AI, and innovation (e.g., blog posts about Knnekt or DevOps tools).

***




## **Benefits**

- **Efficiency**: Reduces manual content creation time by automating title and blog generation.

- **SEO Optimization**: Boosts website visibility with high-ranking titles and search-optimized posts.

- **Scalability**: Easily scale content production for multiple topics or websites.

- **Thought Leadership**: Positions your company or project as a leader in AI and DevOps, attracting business owners and agency leaders.

***


## **Troubleshooting**

- **API Errors**: Ensure your OpenRouter and Webflow API credentials are valid and have sufficient permissions.

- **Webflow Publishing Issues**: Verify the siteId and collectionId match your Webflow setup, and ensure the blog collection fields (“name” and “post-body”) are correctly configured.

- **AI Output Issues**: If the Deepseek AI generates unexpected output, adjust the system messages in the Title Agent and Blog Body Agent nodes for clarity or specificity.

***


## **Future Enhancements**

- Integrate additional AI models (e.g., Grok, Claude) via OpenRouter for variety in content generation.

- Add image generation nodes to include visuals in Webflow posts (while maintaining the no-image requirement in the current workflow).

- Expand to other CMS platforms (e.g., WordPress) or social media posting for broader distribution.

***


## **About the Author**

I’m Sourav Dhiman, a DevOps engineer with expertise in CI/CD, Kubernetes, Docker, AWS, GCP, and AI tools. Passionate about building scalable solutions, I’m currently working on Knnekt—a platform for curated dining experiences—and side projects like this workflow to drive innovation in AI and DevOps. Follow my journey on[ LinkedIn](https://www.linkedin.com/in/dsourav155/) and [Twitter](https://x.com/d_sourav156)to check out my technical blogs.
