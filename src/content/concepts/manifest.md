---
title: 매니페스트
sort: 10
contributors:
  - skipjack
related:
  - title: Separating a Manifest
    url: https://survivejs.com/webpack/optimizing/separating-manifest/
  - title: Predictable Long Term Caching with Webpack
    url: https://medium.com/webpack/predictable-long-term-caching-with-webpack-d3eee1d3fa31
  - title: Caching
    url: /guides/caching
---

웹팩으로 구축된 일반적인 어플리케이션이나 사이트에는 세 가지 주요 유형의 코드가 있습니다.

1. 당신과 당신 팀이 작성한 소스 코드.
2. 소스가 의존하는 타사 라이브러리 또는 "Vendor" 코드.
3. 모든 모듈의 상호 작용을 수행하는 웹팩 런타임 및 _매니페스트_.

이 글에서는 세 부분 중 마지막 부분인 런타임과 매니페스트를 다루며, 특히 매니페스트를 중점적으로 다룹니다.

## 런타임

위에서 언급했듯이, 우리는 런타임에 대해서는 간략하게 다룰 것입니다. 매니페스트 데이터와 함께 런타임은 기본적으로 웹팩이 브라우저에서 실행되는 동안 모듈화 된 어플리케이션을 브라우저에 연결하는데 필요한 모든 코드입니다. 모듈은 상호 작용할 때, 모듈을 연결하는 데 필요한 로딩 및 분석 논리를 포함합니다. 여기에는 브라우저에 이미 로드된 모듈과 그렇지 않은 모듈을 지연-로드하는 로직이 포함됩니다.

## 매니페스트

애플리케이션이  일부 번들 및 기타 다양한 애셋과 함께 `index.html` 파일 형태로 브라우저를 접근한다면 어떻게 될까요? 꼼꼼하게 배치한 `/src` 디렉토리가 사라졌지만, 웹팩은 모든 모듈 간의 상호 작용을 어떻게 관리할까요? 이것은 매니페스트 데이터를 통해 가능해졌습니다.

컴파일러가 어플리케이션을 입력, 분석 및 매핑을 하면, 모든 모듈에 대한 자세한 정보가 유지됩니다. 이 데이터 수집은 "매니페스트(Manifest)"라고 불리며 런타임에 모듈이 번들되고 브라우저로 전달된 후 모듈을 확인하고 로드하는데 사용합니다. 어떤 [module syntax](/api/module-methods)를 선택하든, 그것들의 `import` 또는 `require` 문은 모두 모듈 식별자를 가리키는 `__webpack_require__` 메소드가 됩니다. 매니페스트의 데이터를 사용하면 런타임에서 식별자 뒤에서 모듈을 검색할 위치를 찾을 수 있습니다.

## 문제

이제 웹팩이 배후에서 어떻게 작동하는 지에 대해 조금 이해할 수 있게 되었습니다. 이 과정은 당신에게 거의 영향을 끼치지 않습니다. 런타임은 매니페스트를 사용하여 작업을 수행하며 애플리케이션이 브라우저에 도달하면 모든 것이 마술처럼 작동합니다. 그러나 브라우저 캐싱을 사용하여 프로젝트 성능을 향상시키려는 경우, 이 프로세스를 이해하는 것은 당신에게 보다 중요하게 여겨지게 될 것입니다.

번들 파일 이름에 콘텐츠 해시를 사용하면, 파일의 내용이 변경되어 캐시를 무효화 할 때 브라우저에 표시 할 수 있습니다. 일단 당신이 이것을 시작하게 되면, 당신은 즉시 몇 가지 재미있는 행동을 알아차릴 수 있을 것입니다. 특정 해시는 내용이 분명히 변경되지 않는 경우에도 변경됩니다. 이것은 모든 빌드를 변경하는 런타임 및 매니페스트의 주입으로 인해 발생하게 됩니다.

매니페스트를 추출하는 방법을 배우려면 _Managing Built Files_ 가이드의 [the manifest section](/guides/output-management#the-manifest)를 참조하고, 장기 캐시의 복잡성에 대해 자세히 알아 보려면 아래 가이드를 읽으십시오.