# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a conversational AI demo that integrates ElevenLabs' voice AI with screen sharing and image analysis capabilities. The application allows users to have voice conversations with an AI agent that can analyze their screen in real-time.

## Tech Stack

- **Framework**: Next.js 15.0.2 with TypeScript and App Router
- **UI**: Tailwind CSS, Shadcn/ui components (New York style)
- **AI Integration**: 
  - ElevenLabs SDK for conversational AI
  - OpenRouter API (Mistral model) for image analysis
- **Build**: Turbopack for development

## Key Commands

```bash
# Development
npm run dev       # Start dev server with Turbopack

# Production
npm run build     # Build for production
npm run start     # Start production server

# Code Quality
npm run lint      # Run ESLint
```

## Architecture & Core Components

### Data Flow
1. User initiates conversation → API generates signed URL → WebSocket connection to ElevenLabs
2. Screen sharing captures images every 3 seconds → Stored in component state
3. AI agent calls "SeeImage" tool → Image sent to OpenRouter API for analysis
4. Analysis results integrated into conversation context

### Key Files
- `components/ConvAI.tsx`: Core conversational AI component with screen sharing logic
- `app/api/signed-url/route.ts`: Server endpoint for ElevenLabs authentication
- `app/layout.tsx`: Root layout with navigation and visual effects

### Client Tools System
The AI agent has access to a "SeeImage" tool that:
- Accepts an `image_prompt` parameter describing what to analyze
- Sends the latest captured screen to OpenRouter API
- Returns detailed analysis for the agent to use in conversation

## Environment Variables

Required in `.env`:
```
AGENT_ID=your_elevenlabs_agent_id
ELEVENLABS_API_KEY=your_elevenlabs_api_key  # Automatically used by SDK
```

Required in `.env.local`:
```
NEXT_PUBLIC_OPENROUTER_API_KEY=your_openrouter_api_key
```

## Development Guidelines

### Screen Capture Implementation
- Screen sharing uses `navigator.mediaDevices.getDisplayMedia()`
- Images captured every 3 seconds via canvas rendering
- Latest image stored in component state for AI analysis

### API Integration Patterns
- ElevenLabs: Server-side signed URL generation for security
- OpenRouter: Client-side calls with structured prompts for image analysis
- Error handling with detailed logging for debugging

### State Management
- React hooks for local state (screen sharing, captured images)
- useConversation hook from @11labs/react for conversation state
- Refs for managing media streams and intervals