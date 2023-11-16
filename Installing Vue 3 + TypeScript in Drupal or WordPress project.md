> Installation uses Webpack

## Yarn install

dependencies:

```
yarn add vue @vue/compiler-sfc
```

dev-dependencies:

```
yarn add -D vue-loader typescript ts-loader
```

## Config files

webpack.config.js

```
const path = require('path');
const webpack = require("webpack");
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');
const { VueLoaderPlugin } = require("vue-loader");

// https://stackoverflow.com/questions/66189561/you-are-running-the-esm-bundler-build-of-vue-it-is-recommended-to-configure-you
// conditional settings based on mode
module.exports = (env, argv) => {
  // default config
  const config = {
    devtool: "source-map",
    entry: path.resolve(__dirname, "./src/js/script.js"),
    module: {
      rules: [
        {
          test: /\.vue$/,
          use: ["vue-loader"],
        },
        {
		  test: /\.ts$/,
          loader: 'ts-loader',
          exclude: /node_modules/,
		  options: {
		    apendTsSuffixTo: [/\.vue$/],
		  },
		},
        {
          test: /\.(js)$/,
          exclude: /node_modules/,
          use: ["babel-loader"],
        },
        {
          test: /\.css$/,
          use: ["style-loader", "css-loader"],
        },
      ],
    },
    resolve: {
      alias: {
        vue$: "vue/dist/vue.esm-bundler.js",
      },
      extensions: ["*", ".js", ".ts", ".vue"],
    },
    output: {
      path: path.resolve(__dirname, "dist/js"),
      filename: 'script.js',
      chunkFilename: '[name].[contenthash].js',
      clean: true,
    },
    plugins: [
      new BrowserSyncPlugin({
        proxy: 'drupal-base.ddev.site',
        open:  false,
      }),
      new VueLoaderPlugin(),
    ],
  };

  if (argv.mode === "development") {
    config.plugins.push(
      new webpack.DefinePlugin({
        __VUE_OPTIONS_API__: true,
        __VUE_PROD_DEVTOOLS__: true,
      })
    );
  }

  if (argv.mode === "production") {
    config.plugins.push(
      new webpack.DefinePlugin({
        __VUE_OPTIONS_API__: true,
        __VUE_PROD_DEVTOOLS__: false,
      })
    );
  }

  return config;
};
```

Create `vue.shims.d.ts` to pass Vue types to TS:

```
declare module "*.vue" {
  import { DefineComponent } from "vue";
  const component: DefineComponent<{}, {}, any>;
  export default component;
}
```

Create `tsconfig.json` file with TS configuration:

```
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "outDir": "./dist"
  },
  "include": ["PATH_TO_FILES/**/*"]
}
```

> We need to keep `script.js` and component root JS files in plain JS (not TypeScript). However, the nested component can make use of TS now.

## Common libraries

### VueUse

```
yarn add @vueuse/core
```

## Axios

```
yarn add axios
```
