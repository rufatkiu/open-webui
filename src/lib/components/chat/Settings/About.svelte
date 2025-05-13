<script lang="ts">
	import { getVersionUpdates } from '$lib/apis';
	import { getOllamaVersion } from '$lib/apis/ollama';
	import { WEBUI_BUILD_HASH, WEBUI_VERSION } from '$lib/constants';
	import { WEBUI_NAME, config, showChangelog } from '$lib/stores';
	import { compareVersion } from '$lib/utils';
	import { onMount, getContext } from 'svelte';

	import Tooltip from '$lib/components/common/Tooltip.svelte';

	const i18n = getContext('i18n');

	let ollamaVersion = '';

	let updateAvailable = null;
	let version = {
		current: '',
		latest: ''
	};

	const checkForVersionUpdates = async () => {
		updateAvailable = null;
		version = await getVersionUpdates(localStorage.token).catch((error) => {
			return {
				current: WEBUI_VERSION,
				latest: WEBUI_VERSION
			};
		});

		console.log(version);

		updateAvailable = compareVersion(version.latest, version.current);
		console.log(updateAvailable);
	};

	onMount(async () => {
		ollamaVersion = await getOllamaVersion(localStorage.token).catch((error) => {
			return '';
		});

		checkForVersionUpdates();
	});
</script>

<div class="flex flex-col h-full justify-between space-y-3 text-sm mb-6">
	<div class=" space-y-3 overflow-y-scroll max-h-[28rem] lg:max-h-full">
		<div>
			<div class=" mb-2.5 text-sm font-medium flex space-x-2 items-center">
				<div>
					{$WEBUI_NAME}
				</div>
			</div>
		</div>

		{#if ollamaVersion}
			<hr class=" border-gray-100 dark:border-gray-850" />

			<div>
				<div class=" mb-2.5 text-sm font-medium">{$i18n.t('Ollama Version')}</div>
				<div class="flex w-full">
					<div class="flex-1 text-xs text-gray-700 dark:text-gray-200">
						{ollamaVersion ?? 'N/A'}
					</div>
				</div>
			</div>
		{/if}

		<hr class=" border-gray-100 dark:border-gray-850" />

		{#if $config?.license_metadata}
			<div class="mb-2 text-xs">
				{#if !$WEBUI_NAME.includes('Open WebUI')}
					<span class=" text-gray-500 dark:text-gray-300 font-medium">{$WEBUI_NAME}</span> -
				{/if}
			</div>
		{:else}
		{/if}

		<div>
			<pre
				class="text-xs text-gray-400 dark:text-gray-500">Copyright (c) {new Date().getFullYear()} <a
					href="https://neverforever.space"
					target="_blank"
					class="underline">NeverForever</a
				>
隐维客（Wikiless）
项目定位
隐维客是一个面向隐私保护的 Wikipedia 开源前端，旨在屏蔽用户与 Wikipedia 服务器的直接交互，避免 IP 追踪和数据收集，同时移除一切广告和客户端 JavaScript。用户只需访问隐维客实例，即可无缝查阅维基百科内容，却不会暴露个人信息。 
GitHub

核心功能

无 JavaScript、无广告：完全在服务端渲染，客户端不会加载任何脚本或广告资源。 
GitHub

后端代理请求：所有对 Wikipedia 的查询均由隐维客后端完成，浏览器直接与后端通信，Wikipedia 毫无感知。 
AlternativeTo

自托管支持：开源许可（AGPL-3.0），任何组织或个人都可部署私有实例，保障访问控制和可用性。 
黑客新闻

发展历程

接手来源：最初由芬兰开发团队维护的开源克隆项目，在创始人高三时接手并持续两年开发。

镜像范围：除了 Wikipedia，还包含对 Twitter、Reddit、YouTube、Pixiv 等主流站点的隐私镜像。

奇源空间（NeverForever.space）
项目演进

最初名称：起初名为“夜莺论坛”（yein.pub），随后域名更迭为 sqziyuan.xyz、karstin.net、oooo.fit，最终发展为“奇源空间”（NeverForever.space）。

核心定位：从 HVH 游戏外挂圈起家，向综合性大型黑客论坛转型，提供工具分享、技术交流、外挂倒卖和黑号续费服务。

主要功能与服务

技术交流区：涵盖渗透测试、漏洞利用、逆向工程、脚本编写等模块。

资源交易区：外挂工具、游戏账号、黑产服务一应俱全，并提供会员制付费订阅。

广告与会员盈利：通过论坛广告位和高级会员服务实现商业化。

现状
奇源空间现已成为国内外规模较大的黑客社区之一，注册用户数十万，日活跃度稳居前列。

量核智引（Qorle）
项目起源

前身：“珊瑚搜索”（Azisky），基于 Google CSE 简易爬虫；

演进：升级为 AI 集成搜索引擎“量核智引”（Qorle），日均搜索量数十万次。

功能特色

多源整合：聚合主流搜索引擎和 AI 问答接口，智能分发查询请求并汇总最优结果；

AI 助手：内置问答机器人，提供语义理解和自然语言摘要；

用户定制：支持关键词监控和搜索结果过滤，满足垂直领域需求。

元维联邦（Metastem.su）
项目定位

服务内容：提供短链接、子域名注册与分发服务；

分布式风险管理：通过联邦子域名架构，分散单点故障与审查风险。

业务生态
创始人依托元维联邦子域名，几乎复刻了谷歌、微软等大厂提供的主要互联网服务，包括但不限于：

社交媒体、即时通信

翻译、音乐、视频流媒体

数字地球（地图服务）、虚拟货币

去中心化域名解析

消息推送与收发

所有服务均免费对外开放，收益主要来自广告投放和会员续费。
</pre>
		</div>

		<div class="mt-2 text-xs text-gray-400 dark:text-gray-500">
			{$i18n.t('Created by')}
			<a
				class=" text-gray-500 dark:text-gray-300 font-medium"
				href="https://neverforever.space"
				target="_blank">NeverForever</a
			>
		</div>
	</div>
</div>
