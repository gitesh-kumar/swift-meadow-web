---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2026-01-05
type: landing

design:
  # Default section spacing
  spacing: '0'

sections:
  # Developer Hero - Gradient background with name, role, social, and CTAs
  - block: dev-hero
    id: hero
    content:
      username: me
      greeting: "Hi, I'm"
      show_status: true
      show_scroll_indicator: true
      typewriter:
        enable: true
        prefix: "I build"
        strings:
          - "applied AI systems"
          - "robust data pipelines"
          - "explainable AI models"
          - "industrial digitalization solutions"
        type_speed: 70
        delete_speed: 40
        pause_time: 2500
      cta_buttons:
        - text: View My Work
          url: "#projects"
          icon: arrow-down
        - text: Get In Touch
          url: "#contact"
          icon: envelope
    design:
      style: centered
      avatar_shape: circle
      animations: true
      background:
        color:
          light: "#fafafa"
          dark: "#0a0a0f"
      spacing:
        padding: ["6rem", "0", "4rem", "0"]
  
  # Filterable Portfolio - Alpine.js powered project filtering
  - block: portfolio
    id: projects
    content:
      title: "Featured Projects"
      subtitle: "Bringing Academic Research to Industrial Practice"
      count: 0
      filters:
        folders:
          - projects
      buttons:
        - name: All
          tag: '*'
        - name: Fine-tuned LLMs and RAG
          tag: LLMs
        - name: Computer Vision
          tag: Vision
        - name: Data-Engineering
          tag: Data-Eng
      default_button_index: 0
      # Archive link auto-shown if more projects exist than 'count' above
      # archive:
      #   enable: false  # Set to false to explicitly hide
      #   text: "Browse All"  # Customize text
      #   link: "/work/"  # Custom URL
    design:
      columns: 3
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # Visual Tech Stack - Icons organized by category
  - block: tech-stack
    id: skills
    content:
      title: "Technical Expertise"
      subtitle: "Tools I use to solve complex industrial problems"
      categories:
        - name: AI & Machine Learning
          items:
            - name: Python
              icon: devicon/python
            - name: PyTorch
              icon: devicon/pytorch
            - name: Scikit-learn
              icon: devicon/python # Uses python icon as fallback
            - name: Hugging Face
              icon: devicon/huggingface
        - name: Data & Industrial
          items:
            - name: SQL
              icon: devicon/postgresql
            - name: Docker
              icon: devicon/docker
            - name: Git
              icon: devicon/github
            - name: Linux
              icon: devicon/linux
    design:
      style: grid
      show_levels: false
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # Experience Timeline
  - block: resume-experience
    id: experience
    content:
      title: Professional Experience
      date_format: Jan 2006
      items:
        - title: Sr. Operations Officer (Data & Systems)
          company: Hindustan Petroleum Corporation Limited
          company_url: 'https://www.hindustanpetroleum.com/'
          location: India
          date_start: '2017-07-01'
          date_end: '2021-08-31'
          description: |2-
            * **Digital Modernization:** Architected Python-based ETL pipelines to integrate SCADA and ERP (JD Edwards) data, replacing manual reporting for safety-critical operations.
            * **Systems Integration:** Served as the primary technical coordinator for field-testing and operational validation of industrial vision systems in collaboration with Honeywell.
            * **Operational Impact:** Analyzed production patterns and historical downtime to identify bottlenecks, driving a 10% increase in overall plant efficiency.
            * **Cross-Functional Leadership:** Managed the end-to-end data lifecycle for high-capacity LPG operations, ensuring data integrity for regulatory compliance and safety auditing.
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  



  
  # Contact Section
  - block: contact-info
    id: contact
    content:
      title: Get In Touch
      subtitle: "Open to Applied AI & Systems Engineering opportunities"
      text: |-
        I am always interested in discussing new challenges in Applied AI, data engineering or systems optimization. 
        
        Whether you are looking to hire, collaborate, or simply exchange ideas, feel free to reach out!
      email: giteshkumar15@gmail.com
      autolink: true
    design:
      columns: '1'
      background:
        color:
          light: "#ffffff"
          dark: "#0d0d12"
      spacing:
        padding: ["4rem", "0", "4rem", "0"]
  
  # CTA Card
  - block: cta-card
    content:
      title: "Ready for the Next Challenge"
      text: |-
        I am currently seeking roles where I can apply my experience in **Industrial Systems** and **Applied AI**.
        
        Download my comprehensive Resume below.
      button:
        text: 'Download Resume'
        url: uploads/Resume_Gitesh_Kumar.pdf
        new_tab: true
    design:
      card:
        # Light mode: soft pastel theme gradient | Dark mode: rich deep gradient
        css_class: 'bg-gradient-to-br from-primary-200 via-primary-100 to-secondary-200 dark:from-primary-600 dark:via-primary-700 dark:to-secondary-700'
        text_color: dark
      background:
        color:
          light: "#f5f5f5"
          dark: "#08080c"
      spacing:
        padding: ["4rem", "0", "6rem", "0"]
---
