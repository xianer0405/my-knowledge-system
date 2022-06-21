1. 什么是peerDependencies

 [一文搞懂peerDependencies](https://segmentfault.com/a/1190000022435060)

2. Npm Scripts

s1 && s2  串行

s1 & s1 并行，执行顺序不固定

脚本引用时如何透传参数

Cross-var ? 

Cross-env ?

Scripty 将脚本迁移到文件中

cross-var引用变量规范， 对mac，window,linxu不同语法的兼容性

3. 前置命令和后置命令
package.json

```
{
    "scripts": {
        "pretpub": "# 更新补丁版本号 \n npm version patch",
        "tpub": "# GRAY环境打包和发布 \n npm run build && tnpm publish",
        "posttpub": "# GRAY环境更新版本 \n git add . && git commit -m 'fix: update version' && git push",
    }
}
npm run tpub

当执行tpub命令是， 会先自动执行pretpub, 再执行tpub, 然后执行posttpub命令

```


