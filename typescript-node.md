## Typescript 디펜던시 추가
```bash
# typescript / 빌드용 유틸 추가
npm install --save-dev typescript @types/node concurrently nodemon

# 테스트 환경 (typescript + jest)
npm install --save-dev ts-jest jest @types/jest
```

package.json 수정
```json 
  "scripts": {
    "build": "tsc -p .",
    "build:watch": "tsc -p . --incremental --watch",
    "dev": "npm run build && concurrently \"npm run build:watch\" \"nodemon --watch build build/index.js\"",
    "test": "jest",
  },
```

#### ```<projectDir>/jest.config.js``` 파일 생성 
```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testPathIgnorePatterns: ["<rootDir>/node_modules/", "<rootDir>/.idea/"],
};
```

#### ```<projectDir>/tsconfig.json```파일 생성
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitReturns": true,
    "outDir": "lib",
    "sourceMap": true,
    "strict": true,
    "target": "es2017"
  },
  "compileOnSave": true,
  "include": [
    "src"
  ]
}
```

#### ```<projectDir>/src/index.ts```파일 생성

```typescript
console.log('hello world');
```



```bash
# 개발 시작
npm run dev

# 빌드
npm run build

# 테스트
npm test
```
