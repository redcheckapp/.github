<p align="center">
  <img src="https://github.com/user-attachments/assets/babce0c5-0cc5-4576-8b41-a2b05998c3fa" alt="RedCheck Organization Banner" width="100%"/>
</p>

> An interactive time and task management ecosystem designed to prioritize daily workloads using artificial intelligence.

Welcome to the official GitHub organization for **RedCheck**. Here you will find the source code, architecture, and deployment configurations for the entire platform. 

RedCheck was engineered with a strict focus on performance, frictionless user experience, and scalable software architecture.

<div align="center">
  <table>
    <tr>
      <td align="center"><b>Landing Page (Astro)</b></td>
      <td align="center"><b>Web Application (React)</b></td>
    </tr>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/906e0098-9208-4a4a-a858-fdf76c02f391" alt="RedCheck Landing Page Showcase" width="450"/></td>
      <td><img src="https://github.com/user-attachments/assets/4e2b928e-003c-4845-b41c-0503bd607dc6" alt="RedCheck Platform Showcase" width="450"/></td>
    </tr>
  </table>
</div>

## Repository Ecosystem

To maintain a clean separation of concerns, the RedCheck infrastructure is divided into specialized micro-repositories:

* **[redcheck-backend](https://github.com/redcheckapp/redcheck-backend):** The core API. Built with Java and Spring Boot. Handles stateless JWT authentication, complex business logic, MySQL database interactions, and the prompt engineering bridge with Google Gemini AI models.
* **[redcheck-frontend](https://github.com/redcheckapp/redcheck-frontend):** The main Single Page Application (SPA). Built with React, TypeScript, and Tailwind CSS. Features dynamic agenda views, recurrent task management, and an interactive AI priority board.
* **[redcheck-landing](https://github.com/redcheckapp/redcheck-landing):** The static presentation website. Built with Astro for zero-JS delivery by default and maximum SEO performance. Includes native internationalization (i18n) and smooth DOM animations.

## Core Technology Stack

**Frontend & UI**
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Astro](https://img.shields.io/badge/Astro-0C0E14?style=for-the-badge&logo=astro&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

**Backend & Data**
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white)

**Infrastructure**
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=nginx&logoColor=white)

## Infrastructure Overview

The entire platform is designed to be self-hosted via containerization. 

NGINX acts as the main entry point and reverse proxy. It routes global traffic between the static landing page on the root domain and the React application on the dedicated sub-domain, while securely proxying RESTful API requests to the isolated Spring Boot container network.

```mermaid
graph TD
    %% Node Styles
    classDef client fill:#2d3436,stroke:#636e72,stroke-width:2px,color:#fff;
    classDef proxy fill:#009639,stroke:#00732c,stroke-width:2px,color:#fff;
    classDef frontend fill:#61DAFB,stroke:#00b8d4,stroke-width:2px,color:#000,font-weight:bold;
    classDef static fill:#FF5D01,stroke:#d14d00,stroke-width:2px,color:#fff;
    classDef backend fill:#6DB33F,stroke:#4a8229,stroke-width:2px,color:#fff;
    classDef db fill:#00758F,stroke:#005c70,stroke-width:2px,color:#fff;
    classDef external fill:#4285F4,stroke:#2b66c4,stroke-width:2px,color:#fff;

    %% External Elements
    Users(("Users<br>Browser / Mobile")):::client
    Gemini["Google Gemini AI<br>(External API)"]:::external

    %% VPS Server
    subgraph VPS ["Ubuntu Server (Host)"]
        style VPS fill:none,stroke:#636e72,stroke-width:2px,stroke-dasharray: 5 5

        NGINX["NGINX API Gateway<br>(Ports 80/443 + SSL)"]:::proxy

        %% Docker Internal Network
        subgraph DockerNet ["Internal Network: redcheck-net"]
            style DockerNet fill:none,stroke:#0984e3,stroke-width:2px
            
            Landing["Container: Landing<br>(Astro)"]:::static
            React["Container: Frontend<br>(React + Vite)"]:::frontend
            Spring["Container: Backend<br>(Spring Boot)"]:::backend
            MySQL[("Container: Database<br>(MySQL 8.0)")]:::db
        end
    end

    %% Traffic & Routing Flow
    Users -- "HTTPS" --> NGINX
    
    NGINX -- "redcheckapp.com<br>(+ 301 Redirect from redcheck.es)" --> Landing
    NGINX -- "my.redcheckapp.com" --> React
    
    React -. "API Calls" .-> Spring
    
    %% AI & Data Persistence Flow
    Spring -- "1. Sends Context & Prompt" --> Gemini
    Gemini -. "2. Returns AI Response" .-> Spring
    Spring == "3. Persists AI Data &<br>App State (TCP 3306)" ==> MySQL
```

<p align="center">
  <img src="[INSERT_ARCHITECTURE_DIAGRAM_URL_HERE]" alt="RedCheck Architecture Diagram" width="800"/>
</p>

## Links & Resources

* **Official Website:** [redcheckapp.com](https://redcheckapp.com)
* **Web Application:** [my.redcheckapp.com](https://my.redcheckapp.com)
