<div align="center">
  <img src="./assets/banner.png" width="100%" style="border-radius: 15px;">
</div>

<p align="center">
  <a href="https://discord.gg/6dffbvGU3D">
      <img src="https://dcbadge.vercel.app/api/server/6dffbvGU3D?compact=true&style=flat" alt="Discord">
  </a>
  <a href="https://github.com/RecursivelyAI/CopilotKit/actions/workflows/ci.yml">
      <img src="https://github.com/RecursivelyAI/CopilotKit/actions/workflows/ci.yml/badge.svg" alt="GitHub CI">
  </a>
</p>

<h1 align="center">The open-source copilot platform.</h1>

🌟 **Copilot Textarea:** AI-assisted text generation + editing + autocompletions. <br/>
🌟 **Copilot Chatbot:** in-app chatbot that can see the app's state (local + cloud), can call local + cloud functions (with support for auth), and can talk to 3rd party plugins. 

  <p align="center">
    <br />
    <a href="https://docs.copilotkit.ai" rel="dofollow"><strong>Explore the docs »</strong></a>
    <br />

  <br/>
    <a href="https://github.com/RecursivelyAI/CopilotKit/issues/new?assignees=&labels=bug&projects=&template=bug_report.md&title=">Report Bug</a>
    ·
    <a href="https://github.com/RecursivelyAI/CopilotKit/issues/new?assignees=&labels=feature+request&projects=&template=feature_request.md&title=">Request Feature</a>
    ·
  <a href="https://discord.gg/6dffbvGU3D">Join Our Discord</a>
  </p>
  


## Overview

### CopilotTextarea: AI-assisted text generation + editing.
- ✅ A a drop-in `<textarea />` replacement. Fully customizable visually.
- ✅ AI editing ✨ - "list the client's top 3 pain points from the last call `@GongTranscript`"
- ✅ autocompletions ✨ - Context-aware autocompletions (like in GitHub Copilot / Gmail)
- ✅ App context & 3rd party context with `useMakeCopilotReadable` and `useMakeCopilotDocumentReadable`
- 🚧 First draft ✨ - automatically populate the initial content.
- 🚧 Bold + italics support.
<img width="650" alt="Screenshot 2023-11-27 at 5 57 29 PM" src="https://github.com/RecursivelyAI/CopilotKit/assets/746397/ce64c287-e834-493d-be8c-dabaae03b8a1">

<br/>
<br/>
<br/>

### Copilot Runtime: (Frontend + backend) runtimes for in-app copilots.
- ✅ propagate state with `useMakeCopilotReadable` and `useMakeCopilotDocumentReadable` ("what should I do here?")
- ✅ frontend actions: `useMakeCopilotActionable`
- ✅ User-referenced context using @someContext (including 3rd party)
- ✅ Bring your own model
- 🚧 backend actions with 3rd party authentication
- 🚧 OpenAI _assistants_ api

## Demo
CopilotKit in action.
<p align="center">
  <img src="./assets/demo.gif" width="500" style="border-radius: 15px;">
</p>

**2-min showcase + 2-min implementation tutorial:**


https://github.com/RecursivelyAI/CopilotKit/assets/746397/b0cdf38b-ec5c-4e95-8623-364bafb70907



## Installation

```bash
npm i @copilotkit/react-core @copilotkit/react-ui @copilotkit/react-textarea
```

## Getting started
See quickstart in the [docs](https://docs.copilotkit.ai)

## Backend

You can easily use any backend/LLM - just provide the URL of any OpenAI-comaptible endpoint.
E.g. see this [example implementation for a NextJS app](CopilotKit/examples/next-openai/src/app/api/copilotkit/chat/route.ts).

Then reference this URL in your `CopilotProvider` instantiation:
```typescript
  <CopilotProvider chatApiEndpoint="/api/copilotkit/chat"> {/* Global state & copilot logic. Put this around the entire app */}
    {/* ... */}
  </CopilotProvider>
```


## Examples

### `<CopilotTextarea />`
A drop-in <textarea /> replacement with context-aware Copilot autocompletions.

<p align="center">
  <img src="./assets/CopilotTextarea.gif" width="400" height="400" style="border-radius: 15px;">
</p>

#### Features
1. Customizable `purpose` prompt.
2. Provide arbitrary context to inform autocompletions using `useMakeCopilotReadable`
3. Works with any backend/LLM, using `ChatlikeApiEndpoint`
4. Supports all `<textarea />` customizations


```typescript
import "@copilotkit/react-textarea/styles.css"; // add to the app-global css
import { CopilotTextarea } from "@copilotkit/react-textarea";
import { CopilotProvider } from "@copilotkit/react-core";

// call ANYWHERE in your app to provide external context (make sure you wrap the app with a <CopilotProvider >):
// See below for more features (parent/child hierarchy, categories, etc.)
useMakeCopilotReadable(relevantInformation)
useMakeCopilotDocumentReadable(document)

return (
  <CopilotProvider chatApiEndpoint="/api/copilotkit/chat"> {/* Global state & copilot logic. Put this around the entire app */}
    <CopilotTextarea
      className="p-4 w-1/2 aspect-square font-bold text-3xl bg-slate-800 text-white rounded-lg resize-none"
      placeholder="A CopilotTextarea!"
      autosuggestionsConfig={{
        purposePrompt: "A COOL & SMOOTH announcement post about CopilotTextarea. Be brief. Be clear. Be cool.",
        forwardedParams: { // additional arguments to customize autocompletions
          max_tokens: 25,
          stop: ["\n", ".", ","],
        },
      }}
    />
  </CopilotProvider>
);
```


### Integrate copilot

```typescript
import "@copilotkit/react-ui/styles.css"; // add to the app-global css
import { CopilotProvider } from "@copilotkit/react-core";
import { CopilotSidebarUIProvider } from "@copilotkit/react-ui";

export default function App(): JSX.Element {
  return (
  <CopilotProvider chatApiEndpoint="/api/copilotkit/chat"> {/* Global state & copilot logic. Put this around the entire app */}
      <CopilotSidebarUIProvider> {/* A built-in Copilot UI (or bring your own UI). Put around individual pages, or the entire app. */}

        <YourContent />

      </CopilotSidebarUIProvider>
    </CopilotProvider>
  );
}
```

#### Features
1. Batteries included. Add 2 React components, and your Copilot is live.
2. Customize the built-in `CopilotSidebarUIProvider` UI -- or bring your own UI component.
3. Extremely hackable. Should the need arise, you can define 1st-class extensions just as powerful as `useMakeCopilotReadable`, `useMakeCopilotActionable`, etc.


### Give the copilot read permissions

#### Features
1. Propagate useful information & granular app-state to the Copilot
2. Easily maintain the hierarchical structure of information with `parentId`
3. One call to rule them all: `useMakeCopilotReadable` works both with the sidekick, and with CopilotTextarea.
   - Use the `contextCategories: string[]` param to route information to different places.


```typescript
import { useMakeCopilotReadable } from "@copilotkit/react-core";


function Employee(props: EmployeeProps): JSX.Element {
  const { employeeName, workProfile, metadata } = props;

  // propagate any information copilot
  const employeeContextId = useMakeCopilotReadable(employeeName);

  // Pass a parentID to maintain a hiearchical structure.
  // Especially useful with child React components, list elements, etc.
  useMakeCopilotReadable(workProfile.description(), employeeContextId);
  useMakeCopilotReadable(metadata.description(), employeeContextId);
  
  return (
    // Render as usual...
  );
}

```

### Give the copilot write permissions

```typescript
import { useMakeCopilotActionable } from "@copilotkit/react-core";

function Department(props: DepartmentProps): JSX.Element {
  // ...

  // Let the copilot take action on behalf of the user.
  useMakeCopilotActionable(
    {
      name: "setEmployeesAsSelected",
      description: "Set the given employees as 'selected'",
      argumentAnnotations: [
        {
          name: "employeeIds",
          type: "array", items: { type: "string" }
          description: "The IDs of employees to set as selected",
          required: true
        }
      ],
      implementation: async (employeeIds) => setEmployeesAsSelected(employeeIds),
    },
    []
  );

  // ...
}
```

#### Features
1. Plain typescript actions. Edit a textbox, navigate to a new page, or anythign you can think of.
2. Specify arbitrary input types.


## Near-Term Roadmap

### 📊 Please vote on features via the Issues tab!

### Copilot-App Interaction

- ✅ `useMakeCopilotReadable`: give static information to the copilot, in sync with on-screen state
- ✅ `useMakeCopilotActionable`: Let the copilot take action on behalf of the user
- 🚧 `useMakeCopilotAskable`: let the copilot ask for additional information when needed (coming soon)
- 🚧 `useEditCopilotMessage`: edit the (unsent) typed user message to the copilot (coming soon)
- 🚧 copilot-assisted navigation: go to the best page to achieve some objective.
- 🚧 CopilotCloudKit: integrate arbitrary LLM logic / chains / RAG, using plain code.

### UI components

- ✅ `<CopilotSidebarUIProvider>`: Built in, hackable Copilot UI (optional - you can bring your own UI).
- ✅ `<CopilotTextarea />`: drop-in `<textarea />` replacement with Copilot autocompletions.

### Integrations

- ✅ Vercel AI SDK
- ✅ OpenAI APIs
- 🚧 Langchain
- 🚧 Additional LLM providers

### Frameworks

- ✅ React
- 🚧 Vue
- 🚧 Svelte
- 🚧 Swift (Mac + iOS)

## Contribute

Contributions are welcome! 🎉

[Join the Discord](https://discord.gg/6dffbvGU3D)
[![Discord](https://dcbadge.vercel.app/api/server/6dffbvGU3D?compact=true&style=flat)](https://discord.gg/6dffbvGU3D)
<!-- [![Discord](https://img.shields.io/discord/1122926057641742418.svg)](https://discord.gg/6dffbvGU3D) -->

