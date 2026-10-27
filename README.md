# Qodalis CLI Angular Example

A minimal Angular 22 app that embeds the Qodalis CLI web terminal from the published `@qodalis/*` npm packages.

[![Open in StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/qodalis-solutions/qodalis-cli-angular-example)
[![Open in CodeSandbox](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/p/github/qodalis-solutions/qodalis-cli-angular-example)
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/qodalis-solutions/qodalis-cli-angular-example)
[![Build](https://github.com/qodalis-solutions/qodalis-cli-angular-example/actions/workflows/build.yml/badge.svg)](https://github.com/qodalis-solutions/qodalis-cli-angular-example/actions/workflows/build.yml)
[![npm](https://img.shields.io/npm/v/@qodalis/angular-cli?label=%40qodalis%2Fangular-cli)](https://www.npmjs.com/package/@qodalis/angular-cli)

## What it is

The smallest working setup of [`@qodalis/angular-cli`](https://www.npmjs.com/package/@qodalis/angular-cli): a terminal
filling the page plus a collapsible side panel, with three plugins registered as modules. Use it as a starting point
or to try the CLI in the browser without installing anything.

| Repository | Role |
| --- | --- |
| [web-cli](https://github.com/qodalis-solutions/web-cli) | Source of every `@qodalis/*` package used here: the engine, the Angular/React/Vue wrappers and the plugins |
| [cli-server-node](https://github.com/qodalis-solutions/cli-server-node), [cli-server-dotnet](https://github.com/qodalis-solutions/cli-server-dotnet), [cli-server-python](https://github.com/qodalis-solutions/cli-server-python) | Optional servers for remote commands, files and shells; this example runs without one |
| [cli-server-dashboard](https://github.com/qodalis-solutions/cli-server-dashboard) | Admin dashboard for the servers |
| [mcp-cli](https://github.com/qodalis-solutions/mcp-cli) | Connects Claude Code to a server |
| [pwa-cli](https://github.com/qodalis-solutions/pwa-cli) | The CLI as a PWA and Android app |
| **qodalis-cli-angular-example** | This repository |

## Quick start

```bash
npm install
npm start       # ng serve, http://localhost:4200
npm run build   # production build into dist/demo
```

## Packages

| Package | Purpose |
| --- | --- |
| `@qodalis/angular-cli` | Angular components: `<cli>` (terminal) and `<cli-panel>` (collapsible panel) |
| `@qodalis/cli`, `@qodalis/cli-core` | Engine and shared types (`CliOptions`, `ICliModule`) |
| `@qodalis/cli-guid` | `guid` command |
| `@qodalis/cli-files` | Virtual file system: `ls`, `cd`, `cat`, `grep`, `find` and more |
| `@qodalis/cli-users` | Users, groups and sign-in |

## How it is wired

Plugins are registered as modules. Each plugin exports an `ICliModule`; `src/app.module.ts` hands them to Angular's
dependency injection with `resolveCliModuleProvider`:

```typescript
import { NgModule } from '@angular/core';
import { CliModule, resolveCliProviders, resolveCliModuleProvider } from '@qodalis/angular-cli';
import { guidModule } from '@qodalis/cli-guid';
import { filesModule } from '@qodalis/cli-files';
import { usersModule } from '@qodalis/cli-users';

@NgModule({
    imports: [CliModule],
    providers: [
        resolveCliProviders(),
        resolveCliModuleProvider(guidModule),
        resolveCliModuleProvider(filesModule),
        resolveCliModuleProvider(usersModule),
    ],
})
export class AppModule {}
```

`src/main.ts` bootstraps the standalone `AppComponent` with `importProvidersFrom(AppModule)`, and the component renders
the terminal and the panel:

```html
<cli [options]="options" height="100%" />
<cli-panel [options]="panelOptions" />
```

Modules can also be passed to a component instead of through DI (here `guidModule` and `filesModule` are component
fields):

```html
<cli [modules]="[guidModule, filesModule]" />
```

To add a plugin, install it (`npm install @qodalis/cli-<name>`) and register its module the same way. The available
plugins are listed in the [documentation](https://cli.qodalis.com/docs).

## Links

- [Documentation](https://cli.qodalis.com/docs)
- [web-cli on GitHub](https://github.com/qodalis-solutions/web-cli)
- [`@qodalis` packages on npm](https://www.npmjs.com/org/qodalis)
