---
layout: single
title:  "Visual Studio Code Snippets 설정 방법"
categories: vscode
tag: vscode
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## PHP 단축키 설정  
  
파일 > 기본설정 > 사용자 코드 조각 > 새 코드 조각 > html.json.code-snippets 입력  
```
{
	// Place your GLOBAL snippets here. Each snippet is defined under a snippet name and has a scope, prefix, body and 
	// description. Add comma separated ids of the languages where the snippet is applicable in the scope field. If scope 
	// is left empty or omitted, the snippet gets applied to all languages. The prefix is what is 
	// used to trigger the snippet and the body will be expanded and inserted. Possible variables are: 
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. 
	// Placeholders with the same ids are connected.
	// Example:
	// "Print to console": {
	// 	"scope": "javascript,typescript",
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }

	"PHP Tag": {
		"prefix": "php",
		"body": "<?php $1 ?>"
	},
	"Inline Echo": {
		"prefix": "phpp",
		"body": "<?= $$1; ?>"
	}
}
```  
  
## C언어 단축키 설정  
  
파일 > 기본설정 > 코드 조각 구성 > c.json 에 아래와 같이 작성  
  
```
"Main Function": {
	"prefix": "!main",
	"body": [
		"#include <stdio.h>\n",
		"int main(void)",
		"{\n\t\n",
		"\treturn 0;",
		"}",
	],
	"description": "Snippets About stdio.h main"
},
"Printf Function": {
	"prefix": "pf",
	"body": [
		"printf(\"\");",
	],
	"description": "Snippets About printf();"
}
```  

## C++ 단축키 설정  
```
"Main Function": {
		"prefix": "!main",
		"body": [
			"#include <iostream>",
			"#include <string>",
			"using namespace std;\n",
			"int main(void)",
			"{\n\t\n",
			"\treturn 0;",
			"}",
		],
		"description": "Snippets About main"
	},
	"cout Function": {
		"prefix": "co",
		"body": [
			"cout << \"\";",
		],
		"description": "Snippets About cout << ;"
	},
	"cin Function": {
		"prefix": "ci",
		"body": [
			"cin >> ;",
		],
		"description": "Snippets About cin >> ;"
	}
```  