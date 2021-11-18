## next/jest が next.js 12.0.4 から公開されたので試してみる

"next/jest" is currently experimental. https://nextjs.org/docs/messages/experimental-jest-transformer

### 使い方
```
const nextJest = require('next/jest');
const createJestConfig = nextJest();

createJestConfig()().then((value) => {
  console.log(value);
});
```

### 結果
```
{
  moduleNameMapper: {
    '^.+\\.module\\.(css|sass|scss)$': '{ローカル環境}/node_modules/next/dist/build/jest/object-proxy.js',
    '^.+\\.(css|sass|scss)$': '{ローカル環境}/node_modules/next/dist/build/jest/__mocks__/styleMock.js',
    '^.+\\.(jpg|jpeg|png|gif|webp|avif|svg)$': '{ローカル環境}/node_modules/next/dist/build/jest/__mocks__/fileMock.js'
  },
  testPathIgnorePatterns: [ '/node_modules/', '/.next/' ],
  transform: {
    '^.+\\.(js|jsx|ts|tsx)$': [
      '{ローカル環境}/node_modules/next/dist/build/swc/jest-transformer.js',
     {
        nextConfig: undefined,
        jsConfig: undefined,
        resolvedBaseUrl: undefined,
        isEsmProject: false
      }
    ]
  },
  transformIgnorePatterns: [ '/node_modules/', '^.+\\.module\\.(css|sass|scss)$' ]
}
```

### 最低限の設定はされているみたいなのでカスタムしたいないようを追加する

```
const nextJest = require('next/jest');
const createJestConfig = nextJest();

const customJestConfig = {
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  collectCoverageFrom: [
    '**/*.{js,jsx,ts,tsx}',
    '!**/*.d.ts',
    '!**/node_modules/**',
  ],
  moduleNameMapper: {
    // Handle module aliases
    '^@/components/(.*)$': '<rootDir>/components/$1',
  },
};

module.exports = createJestConfig(customJestConfig)
```
