# URL Shortener

A simple URL shortener built using **Go** that generates shortened URLs and redirects users to the original URLs.

## Features 🚀
- Shorten long URLs to compact, easy-to-share links.
- Redirect users to the original URL when they visit the shortened link.
- In-memory storage for quick lookups.

## Installation & Setup 🛠️
### Prerequisites
- Install [Go](https://go.dev/doc/install) (1.18 or later recommended)

### Clone the Repository
```sh
git clone https://github.com/DineshNehra29/URL-Shortener.git

```

### Run the Server
```sh
go run main.go
```

The server will start at `http://localhost:3001`.

## Usage 💡
### 1️⃣ Shorten a URL
Send a **POST** request to `/shorten` with a JSON body containing the original URL.

#### Using `curl`
```sh
curl -X POST http://localhost:3001/shorten -H "Content-Type: application/json" -d '{"url": "https://example.com"}'
```
#### Response:
```json
{
  "short_url": "http://localhost:3001/redirect/e86b1e5e"
}
```

### 2️⃣ Redirect to the Original URL
Open the generated short URL in a browser:
```
http://localhost:3001/redirect/e86b1e5e
```
It will redirect you to `https://example.com`.

## API Endpoints 🔗
| Method | Endpoint         | Description                 |
|--------|----------------|-----------------------------|
| GET    | `/`            | Root page                  |
| POST   | `/shorten`     | Shorten a URL              |
| GET    | `/redirect/:id` | Redirect to the original URL |

## Deployment (Optional) 🚢
To make your URL shortener available on the internet:
- Use **Ngrok** to expose the local server:
  ```sh
  ngrok http 3001
  ```
- Deploy it to a cloud provider like **AWS, DigitalOcean, or Heroku**.
- Containerize it using **Docker** for easy deployment.

## Contributing 🤝
Feel free to open issues or submit pull requests to improve the project!



