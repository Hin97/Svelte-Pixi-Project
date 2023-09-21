# Introduction

This Svelte project is created by using [SvelteKit](https://kit.svelte.dev/docs/creating-a-project).


## Run the application

If you're trying to run this application on your machine, please clone the repo and run the following codes:

```bash
# Install the required packages
npm install

# Start the server
npm run dev
```
> Highly recommended to run this application on Google Chrome as it is only tested on Google Chrome

## Important packages

PixiJS is used in this project for rendering the mini game.
The version of PixiJS related packages are shown below:

```bash
"@pixi/graphics-extras": "^7.3.0",
"pixi.js": "^7.1.4"
```

The version of pixi.js was suppose to be 7.2.4, but I have downgraded to 7.1.4 to resolve a typescript issue. (See the next section for details)

## Notes

The project is created with typescript checking enabled, and it has issues when compiling the pixi code (with version 7.2.4):
It will get following error in the console when moving the mouse within the broswer.

```bash
Uncaught TypeError: currentTarget.isInteractive is not a function
```

Did research on this error but seems like pixi with version 7.2.4 is incompatible with typescript.
