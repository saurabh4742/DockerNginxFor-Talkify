# Talkify Reverse Proxy & Load Balancer (NGINX - No Upstream)

A production-ready NGINX-based reverse proxy and load balancing solution for [Talkify](https://talkify-io.vercel.app), a real-time chat/video app. This setup avoids the traditional `upstream` block and instead uses NGINX’s built-in `split_clients` directive to dynamically balance user traffic across multiple Vercel-hosted frontend-backend instances.

---

##  Tech Stack

| Technology      | Purpose                                                   |
|-----------------|-----------------------------------------------------------|
| **NGINX**       | Reverse proxy, load balancer, WebSocket handler           |
| **Vercel**      | Hosts multiple deployed instances of the Talkify app      |
| **Docker (Optional)** | Used to containerize the NGINX setup if preferred   |
| **Socket.IO**   | Enables real-time communication via `/socket.io` route    |
| **Next.js**     | Framework used in Talkify for frontend + backend logic    |

---

##  Key Features

- ✅ Load balances traffic across 4 Vercel Talkify deployments using hash-based distribution
- ✅ Handles WebSocket upgrade for real-time Socket.IO events
- ✅ No `upstream` block — ideal for dynamic or serverless targets
- ✅ Transparent header forwarding for origin and real client IP
- ✅ Logs backend usage per request for monitoring/debugging
- ✅ Simple fallback behavior in case of partial failure

---

##  NGINX Behavior Explained

### 🔀 Split-based Load Balancing

```nginx
split_clients "$http_user_agent$remote_addr$request_time" $backend {
    25% talkify-io.vercel.app;
    25% talkify-io3.vercel.app;
    25% talkify-8b6z.vercel.app;
    25% talkify-io2.vercel.app;
}
