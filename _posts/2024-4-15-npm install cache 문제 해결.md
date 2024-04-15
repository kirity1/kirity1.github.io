---
key:
title: '리엑트네이티브 정리'
excerpt: '리엑트네이티브 정리합니다.'
tags: [react-native]
---

```
npm WARN using --force Recommended protections disabled.
npm ERR! code EACCES
npm ERR! syscall unlink
npm ERR! path /Users/gimjun-yeong/.npm/_cacache/index-v5/00/03/9a227757cb5437ea7adb37debe8f461d0ace3b7e7de9750e99ec6cffd22c
npm ERR! errno -13
npm ERR!
npm ERR! Your cache folder contains root-owned files, due to a bug in
npm ERR! previous versions of npm which has since been addressed.
npm ERR!
npm ERR! To permanently fix this problem, please run:
npm ERR!   sudo chown -R 501:20 "/Users/gimjun-yeong/.npm"

npm ERR! A complete log of this run can be found in: /Users/gimjun-yeong/.npm/_logs/2024-04-15T05_31_19_168Z-debug-0.log
```

캐쉬파일이 노드js를 설치하는 중에 뭔가 꼬여서 발생하는 문제이다, 안의 내용을 읽어보니 캐시폴더가 root-owned 파일에 포함돼있다고 한다, 이걸 해결하기 위해서

```
sudo chown -R 501:20 "/Users/사용자 이름/.npm"

npm cache clean --force

npm install
```

를 차례대로 입력하면 된다.

첫째 줄의 내용이 sudo문으로 지금 캐시폴더가 root-owned에 있기 때문에 캐시폴더의 소유권을 바꿔서 캐시를 지우고 새로 다시 npm을 다운받게 한다.

만약 안되면 npm cache verify도 입력해보자 
