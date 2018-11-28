#### --save 和 --save-dev 的区别

npm i 时使用 --save 和 --save-dev，可分别将依赖（插件）记录到 package.json 中的 dependencies 和 devDependencies 下面。

dependencies 下记录的是项目在运行时必须依赖的插件，常见的例如 react jquery 等，即及时项目打包好了、上线了，这些也是需要用的，否则程序无法正常执行。

devDependencies 下记录的是项目在开发过程中使用的插件，例如这里我们开发过程中需要使用 webpack 打包，而在工作中 使用 fis3 打包，但是一旦项目打包发布、上线之后，webpack 和 fis 就都没有用了。