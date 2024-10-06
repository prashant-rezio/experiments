# Dynamic Non-Expiring Link Sharing for Profiles and Portfolios

This document explains the process for implementing dynamic, non-expiring shared links for profiles and property portfolios in a real estate app. The links allow users (agents/investors) to share their profiles/portfolios, which will always reflect the latest updates (e.g., profile picture, portfolio changes) without needing to update the link itself.

## Overview

### Key Features:
- **Dynamic Content**: The shared links always fetch the most up-to-date profile or portfolio data.
- **Non-expiring Links**: The links do not expire and can be accessed indefinitely.
- **Security**: Token-based access control ensures that only valid requests are processed. Rate limiting and DDoS protection ensure that the links are protected against abuse.

### Steps in the Process:
1. **User Shares Profile/Portfolio**: A user shares their profile or portfolio, triggering the creation of a secure link.
2. **Backend Generates Secure Link**: Django backend generates a link with a secure token (UUID or JWT).
3. **Link Shared**: The generated link is shared via email, messaging apps, etc.
4. **Link Access**: When the link is clicked, Firebase Dynamic Links checks whether the Rezio app is installed. If installed, it opens the profile/portfolio in the app; otherwise, it opens in a browser.
5. **Backend Validates Token**: The Django backend validates the token (UUID/JWT) to ensure the link is authorized.
6. **Profile/Portfolio Displayed**: The latest profile or portfolio data is fetched and displayed as a read-only public page.
7. **Profile Updates Automatically Reflected**: Any updates to the profile or portfolio are automatically reflected, without changing the link.
8. **Optional Revocation**: The user can revoke or reset the shared link if necessary.

### Security Measures:
- **Token Encryption**: Secure tokens (UUID or JWT) are used to prevent unauthorized access.
- **Rate Limiting**: Limits the number of requests from a single source to prevent abuse.
- **DDoS Protection**: Protects against distributed denial-of-service attacks using Cloudflare.
- **HTTPS Encryption**: Ensures secure data transfer between the client and server.

## Flowchart

```mermaid
graph TD
    A(User Shares Profile/Portfolio) --> B(Backend Generates Secure Link UUID/JWT)
    B --> C(Link Shared via Email or Messaging)
    C --> D(User Clicks Link)
    D --> E(Firebase Dynamic Links Handles App or Web Redirection)
    E --> F(Backend Validates Token UUID/JWT)
    F --> G(Django Fetches Latest Profile/Portfolio Data)
    G --> H(Public Read-Only Profile/Portfolio Displayed)
    H --> I(Profile Updates Automatically Reflected)
    
    %% Security measures
    F --> J(Token Validation)
    F --> K(Rate Limiting and DDoS Protection)
    F --> L(HTTPS Encryption)

    %% Optional link revocation
    I --> M(User Revokes Link Optional)
