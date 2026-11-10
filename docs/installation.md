# Installation

This page explains how to run Crawrix locally. Crawrix has two parts: a Python + Flask backend (`server/` folder) and a React + TypeScript frontend (Vite, repository root). Both need to be running.

## Requirements

- [Python](https://www.python.org/downloads/)
- [Node.js](https://nodejs.org/) (npm is included)
- [Git](https://git-scm.com/)

## 1. Get the code

```bash
git clone https://github.com/babayaga0x/CRAWRIX-v4.0.0.git
cd CRAWRIX-v4.0.0
```

## 2. Run the backend

Go to the `server` folder and run:

```bash
cd server

# create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# install dependencies
pip install -r requirements.txt

# start the server
python app.py
```

The terminal will show the address where the server is running (by default `http://127.0.0.1:5000`). You can check it with `GET /ping`, which returns `pong`.

## 3. Run the frontend

Open a second terminal, go to the repository root (the folder with `package.json`) and install dependencies:

```bash
npm install
```

### Development mode

```bash
npm run dev
```

Open the address printed in the terminal (usually `http://localhost:5173`). The page reloads automatically when you change the code.

### Production build

```bash
npm run build
npm run preview
```

`build` creates an optimized version of the app in the `dist/` folder, and `preview` serves it locally (usually at `http://localhost:4173`) so you can check it before deploying.

## Connecting the frontend to a local backend

The frontend sends requests to the address set in `src/features/search/searchApi.ts`:

```ts
const API_URL = "https://crawrix-backend.vercel.app/parse";
```

By default this is the production backend, so a local frontend does **not** use your local backend. To use it, change the address to your local one, for example `http://127.0.0.1:5000/parse`.

The backend allows requests only from `http://localhost:5173`, `http://localhost:4173` and `https://crawrix-frontend.vercel.app` (CORS). If you run the frontend on another address, add it to the `origins` list in `server/app.py`.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `python: command not found` | Try `python3` instead of `python`. |
| `ModuleNotFoundError` | Activate the virtual environment and run `pip install -r requirements.txt` again in the `server` folder. |
| Port already in use | Stop the other process using that port and start again. |
| Frontend cannot reach the backend | Make sure the backend is running, `API_URL` points to it, and the frontend address is in the CORS `origins` list. |
| `429 Too Many Requests` | The limit is 10 requests per minute per IP. Wait a minute and try again. |

## Next steps

- [Usage guide](usage.md)
- [API endpoints](api-endpoints.md)
