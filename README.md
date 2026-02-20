# Itera

3D product design iteration tool. Upload a photo or describe what you want — Itera generates an image, turns it into a 3D mesh, then lets you keep iterating with natural language.

## Architecture

```
Text prompt → HuggingFace SD → Image → Meshy/HF → Mesh → 3D Viewer
Image upload                 ↗
Editor edit → merged prompt → new Image → new Mesh (full regeneration)
```

All providers are abstracted behind interfaces. The app runs fully with mock providers during development (no API keys needed).

## Prerequisites

- **Node.js** 18+

## Getting Started

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Environment Variables

Create a `.env.local` file and configure as needed:

| Variable | Default | Description |
|---|---|---|
| `IMAGE_PROVIDER` | `mock` | `mock` or `huggingface` |
| `MESH_PROVIDER` | `mock` | `mock`, `huggingface`, or `meshy` |
| `HF_IMAGE_MODEL` | — | HuggingFace model ID for text-to-image (e.g. `stabilityai/stable-diffusion-xl-base-1.0`) |
| `HF_API_TOKEN` | — | HuggingFace API token (used for both image and mesh generation) |
| `HF_ENDPOINT` | — | HuggingFace Inference Endpoint URL for image-to-3D |
| `MESHY_API_KEY` | — | Meshy AI API key |
| `OPENAI_API_KEY` | — | OpenAI key for intelligent prompt merging (falls back to string concat) |

## Project Structure

```
src/
├── app/                    # Next.js pages + API routes
│   ├── api/
│   │   ├── image-generate/ # Text → Image
│   │   ├── generate/       # Image → Mesh (starts job)
│   │   ├── status/[jobId]/ # Poll mesh job status
│   │   ├── edit/           # Merge description + instruction → new prompt
│   │   ├── export/         # Model export
│   │   └── proxy/          # CORS proxy for external model URLs
│   └── project/editor/     # 3D editor page
├── components/
│   ├── upload/             # Image uploader
│   ├── viewer/             # 3D viewer (React Three Fiber)
│   ├── editor/             # Chat panel
│   └── pipeline/           # Pipeline progress indicator
├── providers/
│   ├── types.ts            # Shared interfaces
│   ├── image-generation/   # Text → Image providers (mock, HuggingFace SD)
│   └── mesh-generation/    # Image → Mesh providers (mock, HuggingFace, Meshy)
├── lib/
│   ├── base64.ts           # Image encoding helpers
│   └── zip-utils.ts        # Extract OBJ from zip
└── stores/
    └── project-store.ts    # Zustand state (model, description, chat, history)
```

## Tech Stack

- **Next.js 16** (App Router)
- **React Three Fiber + drei** (3D rendering)
- **Three.js** (mesh manipulation)
- **Zustand** (state management)
- **Tailwind CSS** (styling)
