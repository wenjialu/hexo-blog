nvm

What:

`nvm` as a mechanism for managing node versions

官方文档：  https://github.com/nvm-sh/nvm



To switch Node.js from `node@4.4.7` to `node@8.9.1` i have used following command:

```js
nvm install v8.9.1
nvm use v8.9.1
```





Problems:

-bash: nvm: command not found



1. Before installing `nvm`, run this in terminal: `touch ~/.bash_profile`
2. After, run this in terminal:
   `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash`
3. **Important...** - DO NOT forget to *Restart* your terminal **OR** use command **`source ~/.nvm/nvm.sh`** (this will refresh the available commands in your system path).
4. In the terminal, use command `nvm --version` and you should see the version







nvm ls 出错

在终端输入：

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```







Q：不识别es6 中的 ... 语法

```
# 代码举例
const opts = {

      binary: typeof data !== 'string',

      mask: !this._isServer,

      compress: true,

      fin: true,

      ...options

    };
```



A：安装 babel 转换插件

babel-transform-object-rest-spread

 npm install --save-dev babel-cli babel-preset-es2015 babel-preset-es2017

```js
npm install babel-preset-stage-0 --save-dev
```

```
npm install @babel/core @babel/register @babel/preset-env --save-dev
```

资源：

https://stackoverflow.com/questions/16904658/node-version-manager-install-nvm-command-not-found

https://www.cnblogs.com/kengsan/p/6399858.html

https://segmentfault.com/q/1010000012941070