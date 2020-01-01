## Next.js 프로젝트 시작  
```bash
npm install --save react react-dom next
mkdir pages

# typescript 추가
npm install --save-dev typescript @types/react @types/node
```

```

package.json 수정
```json 
  "scripts": {`
    "dev": "next",
    "build": "next build",
    "start": "next start"
  },
```

개발 시작
```bash
npm run dev
```

## Page & 링크 만들기
pages 폴더에 js 파일을 만들면 알아서 라우팅

### static routing
```jsx harmony
import Link from 'next/link';

<Link href="/">
  <a>Home</a>
</Link>
```

### dynamic routing
pages/p/[id].js 생성 (test-[id].js 같은건 안됨)
```jsx harmony
<li>
<Link href={"/p/[id]"} as={`/p/${props.id}`}>
  <a>{props.id}</a>
</Link>
</li>
```
## 초기 로딩시 데이터 패칭
오직 페이지가 export 되는 부분에서만 사용 가능
초기 로드시 서버사이드, 렌더링 이후 클라이언트 사이드에서 수행됨
```jsx harmony
const Index = props => (
  // 사용할 때 props.data 등이 연결됨
);

Index.getInitialProps = async function(context /* 라우팅 정보 */) {
  // ...
  // context.query.id
  return {
    data: ...
  }
};

export default Index;
```

## 스타일링 (CSS -> [styled-jsx](https://github.com/zeit/styled-jsx))

### local css (오직 서넝ㄴ됨 컴포넌트에만 적용)
```jsx harmony
<style jsx>{`
  h1,
  a {
   font-family: 'Arial';
  }

  ul {
   padding: 0;
  }

  li {
   list-style: none;
   margin: 5px 0;
  }

  a {
   text-decoration: none;
   color: blue;
  }

   a:hover {
    opacity: 0.6;
  }
`}</style>
```

### global css (전체 css에 적용)
```jsx harmony
<style jsx global>{`
  .markdown {
   font-family: 'Arial';
  }

  .markdown a {
   text-decoration: none;
   color: blue;
  }

  .markdown a:hover {
   opacity: 0.6;
  }

  .markdown h3 {
   margin: 0;
   padding: 0;
   text-transform: uppercase;
  }
`}</style>
```


## deploy
```bash
npm run build && npm run start
```


## Anaylze bundle size
```bash
# package.json 내에 scripts 에 추가하면 편하다.
cross-env ANALYZE=true next build  
# 혹은 client / sever만
cross-env ANALYZE=client next build  
cross-env ANALYZE=server next build  
```

## lazy loading
많은 페이지에서 사용되는 컴포넌트나 모듈은 common으로 묶이게 된다.  
하지만 많이 사용하더라도 너무 큰 것들은 안묶이는게 좋기 때문에 분리하는게 성능상 좋다.  

### lazy module loading
import 함수를 사용하면 async가 되며, 이 모듈은 파일로 분리되어 페이지별로 로드할 필요가 없다..
```jsx harmony
export default async function loadDb() {
  const firebase = await import('firebase/app');
  await import('firebase/database');
}
```

## lazy load components
```jsx harmony
import dynamic from 'next/dynamic';
const Highlight = dynamic(() => import('react-highlight'));

// Highlight 는 일반 컴포넌트 사용하듯 사용하면 된다.
```

## 기타 도움이 될만한 정보
 브라우저 / 서버 모두 사용가능한 fetch [isomorphic-unfetch](https://www.npmjs.com/package/isomorphic-unfetch)

무료 static hosting [ZEIT](https://zeit.co/)

