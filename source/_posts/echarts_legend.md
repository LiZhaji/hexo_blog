---
title: echarts_legend
date: 2020-11-22 22:06:12
tags:
---
<section id="nice" data-tool="mdnice编辑器" data-website="https://www.mdnice.com" style="font-size: 16px; color: black; padding: 0 10px; line-height: 1.6; word-spacing: 0px; letter-spacing: 0px; word-break: break-word; word-wrap: break-word; text-align: left; font-family: Roboto, Oxygen, Ubuntu, Cantarell, PingFangSC-light, PingFangTC-light, 'Open Sans', 'Helvetica Neue', sans-serif;"><h1 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 24px;"><span class="prefix" style="display: none;"></span><span class="content">需求</span><span class="suffix"></span></h1>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">今天分享一个很实用的ECharts图例多行显示的经验。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">如图，要实现的效果如下。图例需要进行显示多行信息并且icon要与第一行持平。</p>
<figure data-tool="mdnice编辑器" style="margin: 0; margin-top: 10px; margin-bottom: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center;"><img src="https://cdn.nlark.com/yuque/0/2020/png/317801/1606046685034-e3f6fc0d-2598-4cb6-80c9-3daaa07aef26.png" alt="效果图" style="display: block; margin: 0 auto; max-width: 100%;"><figcaption style="margin-top: 5px; text-align: center; color: #888; font-size: 14px;">效果图</figcaption></figure>
<h1 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 24px;"><span class="prefix" style="display: none;"></span><span class="content">步骤</span><span class="suffix"></span></h1>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px;"><span class="prefix" style="display: none;"></span><span class="content">显示多行多样式</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">首先，多行显示需要用到<a href="https://echarts.apache.org/zh/option.html#legend.formatter" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">legend.formatter</a>，该配置项支持自定义图例信息， <code style="font-size: 14px; word-wrap: break-word; padding: 2px 4px; border-radius: 4px; margin: 0 2px; color: #1e6bb8; background-color: rgba(27,31,35,.05); font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; word-break: break-all;">\n</code> 用来换行。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">其次，不同的文字显示不同的样式，需要用到<a href="https://echarts.apache.org/zh/option.html#legend.textStyle.rich" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">富文本样式</a>，具体用法如下。</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://my-wechat.mdnice.com/point.png); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #282c34; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #abb2bf; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; padding-top: 15px; background: #282c34; border-radius: 5px;"><span class="hljs-comment" style="color: #5c6370; font-style: italic; line-height: 26px;">// 配置项，只展示关键信息</span>
<span/>
<span/><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">const</span> options = {
<span/>  <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">legend</span>: {
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">formatter</span>: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">function</span> (<span class="hljs-params" style="line-height: 26px;">name</span>) </span>{
<span/>      <span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">return</span> [
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{name|<span class="hljs-subst" style="color: #e06c75; line-height: 26px;">${name}</span>}{percent|30%}`</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">"{num|300}"</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{others|others}`</span>,
<span/>      ].join(<span class="hljs-string" style="color: #98c379; line-height: 26px;">"\n"</span>);
<span/>    },
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">textStyle</span>: {
<span/>      <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">rich</span>: {
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">name</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">12</span>,
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">percent</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#77b9c3"</span>,
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">num</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">20</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#bd8776"</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">padding</span>: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">10</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">others</span>: {},
<span/>      },
<span/>    },
<span/>  },
<span/>};
<span/></code></pre>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">此时出来的效果如下。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">![原理图(https://cdn.nlark.com/yuque/0/2020/png/317801/1606046528612-d78bd61b-b698-4f76-86cb-26fbb4764977.png)</p>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px;"><span class="prefix" style="display: none;"></span><span class="content">图标首行对齐</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">大致上出来了，还剩一个图标的位置问题。通过观察发现，每个图标的总会显示在该图例的中间位置。为了将图标移上去，我们需要将整个图例拉长，也就是需要扩充上部分的长度，可以通过<a href="https://echarts.apache.org/zh/option.html#legend.textStyle.rich..padding" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">padding</a>实现。如下图。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">为了方便调试，可以给每个块添加背景颜色。</p>
<figure data-tool="mdnice编辑器" style="margin: 0; margin-top: 10px; margin-bottom: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center;"><img src="https://cdn.nlark.com/yuque/0/2020/png/317801/1606048586254-d88e1e2f-c9b0-4bc6-8376-48de6bc9a98c.png?x-oss-process=image%2Fresize%2Cw_1500" alt="image.png" style="display: block; margin: 0 auto; max-width: 100%;"><figcaption style="margin-top: 5px; text-align: center; color: #888; font-size: 14px;">image.png</figcaption></figure>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">添加的代码如下。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">注：文档中对 <code style="font-size: 14px; word-wrap: break-word; padding: 2px 4px; border-radius: 4px; margin: 0 2px; color: #1e6bb8; background-color: rgba(27,31,35,.05); font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; word-break: break-all;">legend.textStyle.rich.&lt;style_name&gt;.padding</code> 这里的描述有误，实际结果为<code style="font-size: 14px; word-wrap: break-word; padding: 2px 4px; border-radius: 4px; margin: 0 2px; color: #1e6bb8; background-color: rgba(27,31,35,.05); font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; word-break: break-all;">[下, 右, 上, 左]</code></p>
<blockquote class="multiquote-1" data-tool="mdnice编辑器" style="border: none; display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; border-left: 3px solid rgba(0, 0, 0, 0.4); background: rgba(0, 0, 0, 0.05); color: #6a737d; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px;">
<p style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0px; color: black; line-height: 26px;">文档描述</p>
<p style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0px; color: black; line-height: 26px;"><code style="font-size: 14px; word-wrap: break-word; padding: 2px 4px; border-radius: 4px; margin: 0 2px; color: #1e6bb8; background-color: rgba(27,31,35,.05); font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; word-break: break-all;">padding: [3, 4, 5, 6]</code>：表示 <code style="font-size: 14px; word-wrap: break-word; padding: 2px 4px; border-radius: 4px; margin: 0 2px; color: #1e6bb8; background-color: rgba(27,31,35,.05); font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; word-break: break-all;">[上, 右, 下, 左]</code> 的边距。</p>
</blockquote>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">按照正常思路应该是上右下左，与css保持一致。猜测是代码有误，可以关注该<a href="https://github.com/apache/incubator-echarts/issues/13658" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">issue</a></p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">来看一下新增的代码。如果调整完后图例位置太下面，可以使用 <code style="font-size: 14px; word-wrap: break-word; padding: 2px 4px; border-radius: 4px; margin: 0 2px; color: #1e6bb8; background-color: rgba(27,31,35,.05); font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; word-break: break-all;">top</code> 调整，允许为负值。</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://my-wechat.mdnice.com/point.png); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #282c34; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #abb2bf; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; padding-top: 15px; background: #282c34; border-radius: 5px;"><span class="hljs-comment" style="color: #5c6370; font-style: italic; line-height: 26px;">// 配置项，只展示关键信息</span>
<span/>
<span/><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">const</span> options = {
<span/>  <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">legend</span>: {
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">top</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">-10</span>,
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">formatter</span>: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">function</span> (<span class="hljs-params" style="line-height: 26px;">name</span>) </span>{
<span/>      <span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">return</span> [
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{name|<span class="hljs-subst" style="color: #e06c75; line-height: 26px;">${name}</span>}{percent|30%}`</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">"{num|300}"</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{others|others}`</span>,
<span/>      ].join(<span class="hljs-string" style="color: #98c379; line-height: 26px;">"\n"</span>);
<span/>    },
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">textStyle</span>: {
<span/>      <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">rich</span>: {
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">name</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">12</span>,
<span/>++        padding: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">50</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>], <span class="hljs-comment" style="color: #5c6370; font-style: italic; line-height: 26px;">// 下右上左</span>
<span/>                    backgroundColor: <span class="hljs-string" style="color: #98c379; line-height: 26px;">'gray'</span>
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">percent</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#77b9c3"</span>,
<span/>++        padding: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">50</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">10</span>],
<span/>                    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">backgroundColor</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">'lightgray'</span>
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">num</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">20</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#bd8776"</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">padding</span>: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">10</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">others</span>: {},
<span/>      },
<span/>    },
<span/>  },
<span/>};
<span/></code></pre>
<h2 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px;"><span class="prefix" style="display: none;"></span><span class="content">更好的方法</span><span class="suffix"></span></h2>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">这时，设计师说，你把第一行的百分比给我加个背景颜色。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">那按照上面的做法就有问题了，用来填充的那部分也会被显示出来。</p>
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">这里有个更好的方法，开头添加一个专门的样式用作填充。</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://my-wechat.mdnice.com/point.png); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #282c34; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #abb2bf; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; padding-top: 15px; background: #282c34; border-radius: 5px;"><span class="hljs-comment" style="color: #5c6370; font-style: italic; line-height: 26px;">// 配置项，只展示关键信息</span>
<span/>
<span/><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">const</span> options = {
<span/>  <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">legend</span>: {
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">formatter</span>: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">function</span> (<span class="hljs-params" style="line-height: 26px;">name</span>) </span>{
<span/>      <span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">return</span> [
<span/>++      <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{top|}`</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{name|<span class="hljs-subst" style="color: #e06c75; line-height: 26px;">${name}</span>}{percent|30%}`</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">"{num|300}"</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{others|others}`</span>,
<span/>      ].join(<span class="hljs-string" style="color: #98c379; line-height: 26px;">"\n"</span>);
<span/>    },
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">textStyle</span>: {
<span/>      <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">top</span>: {
<span/>++      padding: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">40</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>]          
<span/>      },
<span/>      <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">rich</span>: {
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">name</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">12</span>,
<span/>--        padding: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">50</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">percent</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#77b9c3"</span>,
<span/>--        padding: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">50</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">backgroundColor</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">'lightgray'</span>
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">num</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">20</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#bd8776"</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">padding</span>: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">10</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">others</span>: {},
<span/>      },
<span/>    },
<span/>  },
<span/>};
<span/></code></pre>
<h1 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 24px;"><span class="prefix" style="display: none;"></span><span class="content">源码</span><span class="suffix"></span></h1>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://my-wechat.mdnice.com/point.png); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #282c34; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #abb2bf; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; padding-top: 15px; background: #282c34; border-radius: 5px;"><span class="hljs-comment" style="color: #5c6370; font-style: italic; line-height: 26px;">// 配置项，只展示图例信息</span>
<span/>
<span/><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">const</span> options = {
<span/>  <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">legend</span>: {
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">left</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">200</span>,
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">top</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">150</span>,
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">data</span>: [<span class="hljs-string" style="color: #98c379; line-height: 26px;">"直接访问"</span>, <span class="hljs-string" style="color: #98c379; line-height: 26px;">"邮件营销"</span>],
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">formatter</span>: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">function</span> (<span class="hljs-params" style="line-height: 26px;">name</span>) </span>{
<span/>      <span class="hljs-keyword" style="color: #c678dd; line-height: 26px;">return</span> [
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{top|}`</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{name|<span class="hljs-subst" style="color: #e06c75; line-height: 26px;">${name}</span>}{percent|30%}`</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">"{num|300}"</span>,
<span/>        <span class="hljs-string" style="color: #98c379; line-height: 26px;">`{others|others}`</span>,
<span/>      ].join(<span class="hljs-string" style="color: #98c379; line-height: 26px;">"\n"</span>);
<span/>    },
<span/>    <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">textStyle</span>: {
<span/>      <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">rich</span>: {
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">top</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">padding</span>: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">40</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">name</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">12</span>,
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">percent</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#77b9c3"</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">backgroundColor</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"lightgray"</span>,
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">num</span>: {
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">fontSize</span>: <span class="hljs-number" style="color: #d19a66; line-height: 26px;">20</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">color</span>: <span class="hljs-string" style="color: #98c379; line-height: 26px;">"#bd8776"</span>,
<span/>          <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">padding</span>: [<span class="hljs-number" style="color: #d19a66; line-height: 26px;">10</span>, <span class="hljs-number" style="color: #d19a66; line-height: 26px;">0</span>],
<span/>        },
<span/>        <span class="hljs-attr" style="color: #d19a66; line-height: 26px;">others</span>: {},
<span/>      },
<span/>    },
<span/>  },
<span/>};
<span/></code></pre>
<hr data-tool="mdnice编辑器" style="height: 1px; margin: 0; margin-top: 10px; margin-bottom: 10px; border: none; border-top: 1px solid black;">
<p data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">欢迎关注我的其他账号～</p>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; color: black; list-style-type: disc;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; color: rgb(1,1,1); font-weight: 500;"><p style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">博客：http://blog.escript.cn/</p>
</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; color: rgb(1,1,1); font-weight: 500;"><p style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">掘金：<a href="https://juejin.im/user/1697301686129127" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">猪是倒着读着</a></p>
</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; color: rgb(1,1,1); font-weight: 500;"><p style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">知乎：<a href="https://www.zhihu.com/people/zhu-shi-dao-zhao-du-de" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">猪是倒着读着</a></p>
</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; color: rgb(1,1,1); font-weight: 500;"><p style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black;">公众号：炸鸡呀（更多内容）</p>
</section></li></ul>
<figure data-tool="mdnice编辑器" style="margin: 0; margin-top: 10px; margin-bottom: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center;"><img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5379eb07cc84cfd850afb4e41f41acc~tplv-k3u1fbpfcp-zoom-1.image?imageslim" alt style="display: block; margin: 0 auto; max-width: 100%;"></figure>
<p id="nice-suffix-juejin-container" class="nice-suffix-juejin-container" data-tool="mdnice编辑器" style="font-size: 16px; padding-top: 8px; padding-bottom: 8px; margin: 0; line-height: 26px; color: black; margin-top: 20px !important;">本文使用 <a href="https://mdnice.com/?from=juejin" style="text-decoration: none; color: #1e6bb8; word-wrap: break-word; font-weight: bold; border-bottom: 1px solid #1e6bb8;">mdnice</a> 排版</p></section>