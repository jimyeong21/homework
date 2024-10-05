# 2차 과제 - Avatars

## 이미지 성능 최적화하기

- 기존에 제공해주신 jpg 형식의 파일을 webp 형식으로 변환하여 사용해보았습니다.
- &lt;img&gt; 태그에 loading="lazy" 속성을 추가하여 이미지를 지연로딩 시키는 방법도 고려해 보았지만,<br />
  PC 사이즈를 기준으로 하였을 때 페이지 스크롤이 발생하지 않으므로 사용하지 않는 것으로 결정했습니다.

## 아바타의 상태 정보 제공하기

> 방법 1.

```
<ul class="avatars-box">
  <li>
    <img src="./../assets/images/face1.webp" alt="영자" />
    <span class="sr-only">접속 상태</span>
    <span class="status off" aria-label="offline"></span>
  </li>
  <li>
    <img src="./../assets/images/face2.webp" alt="정숙" />
    <span class="sr-only">접속 상태</span>
    <span class="status on" aria-label="online"></span>
  </li>
  .
  .
</ul>
```

- '접속 상태'라는 텍스트를 화면에 보이지는 않지만 스크린 리더기는 읽을 수 있도록 css로 처리한 후,<br/>
  다음으로 오는 &lt;span&gt;태그에 aria-label=".." 속성을 추가하여 현재 접속 상태를 확인할 수 있도록 해보았습니다.
- 한가지 의문인 것은 콘텐츠가 없는 빈 태그에 aria-label 속성을 추가해도 되는가 였습니다. <br /> 공식 문서를 뒤져보았지만 해당 정보는 명시되어있지 않은 것 같았습니다. (저의 짧은 영어 탓일 수도 있습니다.)

> 방법 2.

```
<div role="group" aria-labelledby="now-status">
  <p id="now-status" class="sr-only">현재 사용자들의 접속 상태</p>
  <div class="avatars-list">
    <img src="./../assets/images/face1.webp" alt="영자" />
    <span class="status off" aria-label="offline"></span>
  </div>
  <div class="avatars-list">
    <img src="./../assets/images/face2.webp" alt="정숙" />
    <span class="status off" aria-label="offline"></span>
  </div>
  .
  .
</div>
```

- &lt;div&gt; 태그에 role="group" 속성과 aria-labelledby=".." 속성을 추가하기.
- 위 1번의 경우 &lt;span class="sr-only"&gt;접속상태&lt;/span&gt; 텍스트를 사람마다 매번 읽어주게 되어 사용자에게 피로함을 줄 수도 있지 않을까라는 생각에 고려해본 방법입니다.
- 그러나 &lt;div&gt; 태그를 남발하는 것 같아서 괜한 거부감이 들었습니다.

> 결론

- 다양한 경우의 수를 고려하여 올바른 웹접근성을 구현하는 것이 생각보다 쉽지 않다고 느꼈습니다.
- 완벽한 방법이라고는 생각되지 않지만 1번을 선택하여 정보를 제공하였습니다.

## 레이아웃 구현하기

> float을 사용하여 레이아웃 구현하기

- '띄우다'라는 뜻을 가진 float는 이름처럼 해당 요소를 띄워서 배치해줍니다.
- float: left 속성을 사용하여 리스트가 왼쪽 부터 가로로 배치되도록 하였습니다.
- 그러나 떠있는 요소들의 높이값을 인지하지 못하고 부모 영역의 밖으로 삐져나오거나,<br/> 다음으로 오는 요소들이 어설프게 배치되는 현상이 발생합니다.
- 해결방법으로 부모에게 가상요소 ::after를 지정하여 clear:both로 float을 지워줍니다.
- margin 속성과 child 선택자를 활용하여 li요소들 간의 간격을 20px로 지정하였습니다.

> flex를 사용하여 레이아웃 구현하기

- 부모에게 display: flex 속성을 부여하면 flex-direction 속성의 기본값은 row이기에 <br/>자식 요소들이 가로로 배치됩니다.
- 나열된 8개의 li들은 부모에게 추가된 flex-wrap: wrap 속성으로 인해<br /> 박스 영역의 크기에 맞춰서 자동으로 줄바꿈되어 내려갑니다.
- gap: 20px 속성을 추가하여 li요소들 간의 간격을 설정하였습니다.
