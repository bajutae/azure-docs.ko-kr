---
title: 지도에 HTML 표식 추가 | Microsoft Azure 맵
description: 이 문서에서는 Microsoft Azure Maps 웹 SDK를 사용 하 여 지도에 HTML 마커를 추가 하는 방법에 대해 설명 합니다.
author: Philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 30d9cf9848a8d99505bf9f9c97bef5aaa31065ee
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83119287"
---
# <a name="add-html-markers-to-the-map"></a>맵에 HTML 표식 추가

이 문서에서는 맵에 이미지 파일과 같은 사용자 지정 HTML을 추가하는 방법을 보여줍니다.

> [!NOTE]
> HTML 표식은 데이터 원본에 연결하지 않습니다. 대신 위치 정보가 표식에 직접 추가되고 표식이 [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)인 맵 `markers` 속성에 추가됩니다.

> [!IMPORTANT]
> WebGL을 사용하여 렌더링하는 Azure Maps 웹 컨트롤의 대부분 계층과는 달리, HTML 표식은 렌더링에 기존 DOM 요소를 사용합니다. 따라서 더 많은 HTML 표식이 페이지에 추가 되 고 더 많은 DOM 요소가 있습니다. HTML 표식을 매우 많이 추가하고 나면 성능이 저하될 수 있습니다. 더 큰 데이터 세트의 경우 데이터를 클러스터링하거나 기호 또는 거품형 계층을 사용하는 것이 좋습니다.

## <a name="add-an-html-marker"></a>HTML 표식 추가

[Htmlmarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) 클래스에는 기본 스타일이 있습니다. 표식의 색 및 텍스트 옵션을 설정하여 표식을 사용자 지정할 수 있습니다. HTML 마커 클래스의 기본 스타일은 및 자리 표시 자가 있는 SVG 템플릿입니다 `{color}` `{text}` . 빠른 사용자 지정을 위해 HTML 표식 옵션에서 색 및 텍스트 속성을 설정 합니다. 

다음 코드에서는 HTML 표식을 만들고 color 속성을 "DodgerBlue"로 설정 하 고 text 속성을 "10"으로 설정 합니다. 팝업은 표식에 연결 되 고 `click` 이벤트는 팝업의 표시 여부를 전환 하는 데 사용 됩니다.

```javascript
//Create an HTML marker and add it to the map.
var marker = new atlas.HtmlMarker({
    color: 'DodgerBlue',
    text: '10',
    position: [0, 0],
    popup: new atlas.Popup({
        content: '<div style="padding:10px">Hello World</div>',
        pixelOffset: [0, -30]
    })
});

map.markers.add(marker);

//Add a click event to toggle the popup.
map.events.add('click',marker, () => {
    marker.togglePopup();
});
```

다음은 위의 기능을 실행 하는 전체 코드 샘플입니다.

<br/>

<iframe height='500' scrolling='no' title='맵에 HTML 표식 추가' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)의 펜 <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>맵에 HTML 표식 추가</a>를 참조하세요.
</iframe>

## <a name="create-svg-templated-html-marker"></a>SVG 템플릿 기반 HTML 표식 만들기

Html 표식의 기본값 `htmlContent`는 위치 폴더 `{color}` 및 `{text}`가 있는 SVG 템플릿입니다. 사용자 지정 SVG 문자열을 만들고, 이러한 동일한 자리 표시자를 SVG에 추가할 수 있습니다. 표식의 `color` 및 `text` 옵션을 설정하여 사용자의 SVG에 이러한 자리 표시자를 업데이트합니다.

<br/>

<iframe height='500' scrolling='no' title='사용자 지정 SVG 템플릿을 사용하는 HTML 표식' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)의 펜 <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>사용자 지정 SVG 템플릿을 사용하는 HTML 표식</a>을 참조하세요.
</iframe>

> [!TIP]
> Azure Maps 웹 SDK는 HTML 표식과 함께 사용할 수 있는 여러 SVG 이미지 템플릿을 제공 합니다. 자세한 내용은 [이미지 템플릿 사용 방법](how-to-use-image-templates-web-sdk.md) 문서를 참조 하세요.

## <a name="add-a-css-styled-html-marker"></a>CSS 스타일 HTML 표식 추가

HTML 표식의 이점 중 하나는 CSS를 사용하여 얻을 수 있는 여러 유용한 사용자 지정 기능입니다. 이 샘플에서 HtmlMarker의 콘텐츠는 위치 및 펄스로 고정되는 애니메이션 핀을 만드는 HTML 및 CSS로 구성됩니다.

<br/>

<iframe height='500' scrolling='no' title='HTML DataSource' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)에 의한 Pen <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML DataSource</a>를 참조하세요.
</iframe>

## <a name="draggable-html-markers"></a>드래그할 수 있는 HTML 표식

이 샘플에서는 HTML 표식을 드래그하는 방법을 보여줍니다. HTML 마커 `drag` 는, `dragstart` 및 이벤트를 지원 `dragend` 합니다.

<br/>

<iframe height='500' scrolling='no' title='드래그할 수 있는 HTML 표식' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)의 펜 <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>드래그할 수 있는 HTML 표식</a>을 참조하세요.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>HTML 표식에 마우스 이벤트 추가

이러한 샘플은 HTML 표식에 마우스를 추가하고 이벤트를 드래그하는 방법을 보여 줍니다.

<br/>

<iframe height='500' scrolling='no' title='HTML 표식에 마우스 이벤트 추가' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)의 펜 <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>HTML 표식에 마우스 이벤트 추가</a>를 참조하세요.
</iframe>

## <a name="next-steps"></a>다음 단계

이 문서에서 사용된 클래스 및 메서드에 대해 자세히 알아봅니다.

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

맵에 추가할 더 많은 코드 예제를 보려면 다음 문서를 참조하세요.

> [!div class="nextstepaction"]
> [이미지 템플릿을 사용하는 방법](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [기호 계층 추가](./map-add-pin.md)

> [!div class="nextstepaction"]
> [거품형 계층 추가](./map-add-bubble-layer.md)
