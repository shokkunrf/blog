---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
tags:
  - ""
archives:
  - "{{ now.Format "2006" }}"
  - "{{ now.Format "2006-01" }}"
---
