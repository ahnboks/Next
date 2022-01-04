# Next.js란?
nextjs란 React의 서버사이드 렌더링(SSR)을 쉽게 구현할 수 있게 도와주는 프레임 워크이다.  
(React에서도 SSR 구현이 가능하지만, 구현 방식이 복잡하고 번거롭다.)
<br/><br/>

## 렌더링(Rendering)
서버에서 요청해서 받은 내용을 브라우저에 표시해주는 것

## 1. SSR & CSR
### 1.1 서버사이드 렌더링(SSR)
> Server Side Rendering 의 약자로, 모든 템플릿은 서버 연산을 통해서 렌더링하고 완성된 페이지 형태로 응답한다.  
페이지를 이동할 때마다 새로운 페이지를 요청하며, 이로인해 페이지 새로고침이 발생한다.
> > #### 1.1.1장점
> > * 검색엔진 최적화 (SEO) 가능
> > * 성능개선
> >   - 첫 렌더링 된 html을 클라이언트에게 응답하기에 초기로딩속도를 많이 줄여준다. 
> >   - 자바스크립트 파일을 불러오고 렌더링 작업이 완료되기 이전에 유저가 사이트를 확인할 수 있다.
> > #### 1.1.2 단점
> > * 프로젝트의 복잡도
> > * 다른 페이지를 이동할 때마다 전체 페이지를 재요청해야 하기때문에 서버렌더링에 따른 부하가 발생한다.

### 1.2 클라이언트 사이드 렌더링(CSR)
> Client Side Rendering 의 약자로. 처음 요청시 **한페이지**만 불러온 후, 클라이언트에서 렌더링하는 방식. (SPA: Single Page Application)
> > #### 1.2.1 장점
> > * 첫요청시 한페이지만 불러온 후, 필요한 부분만 리렌더링 하기때문에 SSR보다 빠르다.
> > * 즉, 리로딩 없이 서버로부터 데이터를 받아 화면 갱신이 가능하다.
> > * 코드의 재사용성이 높아진다.
> > #### 1.2.2 단점
> > * 첫페이지 로딩시 모든 파일을 내려받기 때문에 초기로딩속도가 느리다.
> > * 검색엔진 최적화가 어렵다.

<br/>

## 2. Next.js 사용이유
* CSR의 긴 초기로딩속도 개선
  - 서버에서 html모두 렌더링한 후 클라이언트로 보내주기 때문에, CSR보다 구동속도가 빠르게 보여진다.
* SEO 문제
  - CSR의 경우 검색엔진이 js를 읽게되는데, SSR의 경우 서버 측에서 html, css, js를 만들어 컨텐츠를 직접 업로드 함으로써 검색엔진이 게시글이 걸리게 된다.
  - 페이지를 변경할 땐 CSR방식으로 처리하기 때문에 SPA의 장점도 유지할 수 있다.
<br/>

## 3. Next.js의 주요 기능
* SSR : 서버사이드 렌더링
* Pre-rendering : 모든페이지를 html을 미리 만들어둔다(pre-render).
* hot reloading : 개발 중 저장되는 코드는 자동으로 새로고침된다.
* automatic routing : 페이지 기반의 클라이언트 사이드 라우팅. pages 폴더에 있는 파일은 해당 파일 이름으로 라우팅된다. (pages/page1.tsx -> localhost:3000/page1)
* code splitting : 필요에 따라 파일을 불러올 수 있게 여러 개의 파일을 분리 하는 것. 자바스크립트 로딩 시간 개선이 가능하다.

### 3.1 Pre-rendering
>  Next.js는 pre-rendering을 **Static Generation**, **Server-side Rendering** 두 가지 형식으로 제공한다. 
>  
>  * Static Generation(SSG): 빌드 타임에 HTML을 생성하는 pre-rendering 방법이다. pre-rendered된 HTML은 각 요청마다 재사용이 가능하다.
>  * Server-side Rendering(SSR): 각 요청마다 HTML을 생성하는 pre-rendering 방법이다.
>  
>  <code>npm run dev</code>나 <code>yarn dev</code>를 통해 개발을 하는 동안에는 Static Generation 방식을 사용하고 있다해도 모든 페이지가 각 요청마다 pre-rendering 된다.  
>  여기서 중요한 점은, Next.js는 **각 페이지마다 우리에게 어떠한 pre-rendering 방식을 사용할지 선택할 수 있게 해주는 것**이다.  
>  따라서 특정 페이지에는 Static Genration 방식을, 나머지 페이지에는 Server-side Rendering 방식을 사용할 수 있는 **"hybrid"앱**을 만들 수 있다.
>  
>  ### 3.1.1 Static Generation v.s. Server-side Rendering
>  Next.js 공식 문서에서는 가능하면 Static Generation 방식을 사용하기를 권장하고 있다.  
>  페이지가 한번 빌드되어 CDN에 의해 제공이 될 수 있으며 매번 페이지 요청을 하는 것보다 렌더링하는 속도가 빠르기 때문이다.  
>  페이지를 제작할 때 "사용자의 요청보다 먼저 페이지가 렌더링되어도 되는가?" 에 대한 질문을 자신에게 던져보고, 대답이 "응" 이라면 Static Generation을 선택하는게 좋을 듯하다.
>  반대로, 데이터의 업데이트가 많거나 사용자의 요청마다 매번 바뀌는 페이지라면 별로 좋은 선택이 아닐 것이다.   
>  
>  ### getStaticProps
>  > **SSG (Static Site Generation) 개념이다.**   
>  > **"빌드 시에 딱 한 번"** 만 호출되고, 바로 static file로 빌드되며 따라서, 이후 수정이 불가능합니다.   
>  > 앱 빌드 후 고정된 내용이 있는 page에만 사용하는 것이며 좋으며,   
>  > 호출 시마다 매번 data fetch를 하지 않으니 getServerSideProps보다 성능면에서 좋다   
>  
>  ### getServerSideProps
>  > **SSR (Server Side Rendering) 개념이다.**   
>  > **"page가 요청받을때마다"** 호출되어 pre-rendering하며,   
>  > pre-render가 꼭 필요한 **동적 데이터**가 있는 page에 사용하면 된다.   
>  > 매 요청마다 호출되므로 성능은 떨어지나, 내용을 언제든 동적으로 수정 가능하다.   


<br/>

## Next.js 시작하기
```js
1. 프로젝트 생성
npx create-next-app project-name

// typescript
npx create-next-app project-name --typescript

2. 설치 완료 후 개발서버 실행
npm run dev
```

[참고링크]
- SSR, CSR 그림 (https://velog.io/@kdo0129/SSR-CSR)
- SSR, CSR 데이터 흐름(https://www.sarah-note.com/%ED%81%B4%EB%A1%A0%EC%BD%94%EB%94%A9/posting2/)
- Next.js 공식문서 (https://nextjs.org/docs/getting-started)
- webpack 번들링 개념 및 사용이유 (https://humanwater.tistory.com/2)

