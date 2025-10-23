
# React: Google Maps JavaScript API
**gitmint Modfied**


[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/visgl/react-google-maps/tree/main/LICENSE)

นี่คือไลบรารี TypeScript / JavaScript สำหรับใช้งาน Google Maps JavaScript API ร่วมกับแอป React ของคุณ
โดยมีคอลเล็กชันของคอมโพเนนต์ React สำหรับสร้างแผนที่ (maps), จุดปักหมุด (markers) และหน้าต่างข้อมูล (infowindows)
รวมถึงชุด hooks เพื่อใช้ Services และ Libraries ของ Maps JavaScript API [Services][gmp-services] และ [Libraries][gmp-libraries]

## การติดตั้ง

ไลบรารีนี้มีให้ติดตั้งผ่าน npm ในชื่อแพ็กเกจ [`@vis.gl/react-google-maps`][npm-package]

```sh
npm install @vis.gl/react-google-maps
```

หรือ

```sh
yarn add @vis.gl/react-google-maps
```

_(หมายเหตุสำหรับผู้ใช้ PowerShell: เนื่องจากตัวอักษร `@` มีความหมายพิเศษใน PowerShell ให้ใส่ชื่อแพ็กเกจไว้ในเครื่องหมายคำพูด)_

## วิธีใช้งาน (สรุป)

นำเข้า [`APIProvider`][api-provider] แล้วห่อนรอบคอมโพเนนต์ทั้งหมดที่ต้องการให้เข้าถึง Maps JavaScript API
คอมโพเนนต์ใด ๆ ที่อยู่ภายในบริบทของ `APIProvider` จะสามารถใช้ hooks และคอมโพเนนต์จากไลบรารีนี้ได้

ตัวอย่างการแสดงแผนที่อย่างง่าย: ใส่คอมโพเนนต์ [`Map`][api-map] ภายใน `APIProvider` แล้วเพิ่มคอมโพเนนต์อื่น ๆ เช่น
[`Marker`][api-marker], [`AdvancedMarker`][api-adv-marker] หรือ [`InfoWindow`][api-infowindow] เพื่อแสดงเนื้อหาบนแผนที่

ถ้าต้องการทำงานซับซ้อนขึ้น สามารถเพิ่มคอมโพเนนต์ที่ใช้งาน `google.maps.OverlayView` หรือ `google.maps.WebGlOverlayView` ได้เช่นกัน

```tsx
import {AdvancedMarker, APIProvider, Map} from '@vis.gl/react-google-maps';

function App() {
  const position = {lat: 53.54992, lng: 10.00678};

  return (
    <APIProvider apiKey={'YOUR API KEY HERE'}>
      <Map defaultCenter={position} defaultZoom={10} mapId="DEMO_MAP_ID">
        <AdvancedMarker position={position} />
      </Map>
    </APIProvider>
  );
}

export default App;
```

ดูเอกสารเพิ่มเติมใน [documentation][docs] หรือดูตัวอย่างการใช้งานใน [examples][] สำหรับคำอธิบายเชิงลึก

### การใช้งาน Libraries อื่น ๆ ของ Maps JavaScript API

นอกจากการแสดงผลแผนที่แล้ว Maps JavaScript API ยังมีไลบรารีเพิ่มเติมสำหรับงานอย่างการแปลงที่อยู่เป็นพิกัด (geocoding),
การวางแผนเส้นทาง (routing), Places API, Street View และอื่น ๆ อีกมากมาย

ไลบรารีเหล่านี้จะไม่ถูกโหลดโดยค่าเริ่มต้น ดังนั้นโมดูลนี้จึงมี hook [`useMapsLibrary()`][api-use-lib]
เพื่อช่วยโหลดไลบรารีเพิ่มเติมแบบไดนามิก

ตัวอย่าง: หากคุณต้องการใช้คลาส `google.maps.geocoding.Geocoder` ในคอมโพเนนต์ แต่ไม่ต้องการแสดงแผนที่เลย
สามารถทำได้ดังนี้:

```tsx
import {useMap, useMapsLibrary} from '@vis.gl/react-google-maps';

const MyComponent = () => {
  // useMapsLibrary จะโหลดไลบรารี geocoding ให้ ซึ่งในตอนแรกอาจคืนค่า `null`
  // หากยังไม่ได้โหลด เมื่อโหลดเสร็จจะคืนค่าอ็อบเจ็กต์ไลบรารี เหมือนกับที่ได้จาก `await google.maps.importLibrary()`
  const geocodingLib = useMapsLibrary('geocoding');
  const geocoder = useMemo(
    () => geocodingLib && new geocodingLib.Geocoder(),
    [geocodingLib]
  );

  useEffect(() => {
    if (!geocoder) return;

    // ตอนนี้คุณสามารถเรียกใช้ `geocoder.geocode(...)` ได้ที่นี่
  }, [geocoder]);

  return <></>;
};

const App = () => {
  return (
    <APIProvider apiKey={'YOUR API KEY HERE'}>
      <MyComponent />
    </APIProvider>
  );
};
```

## ตัวอย่าง

สำรวจโฟลเดอร์ [examples directory on GitHub](./examples) หรือตัวอย่างบนเว็บไซต์ของเราที่ [examples][examples] เพื่อดูตัวอย่างการใช้งานแบบครบถ้วน

## เบราว์เซอร์ที่รองรับ

เนื่องจากไลบรารีนี้สร้างขึ้นบนพื้นฐานของ Google Maps JavaScript API เราจึงปฏิบัติตามนโยบายการรองรับเบราว์เซอร์ของทีม Google Maps
ดูข้อมูลได้ที่หน้า [browser support][gmp-browsersupport]
โดยทั่วไปจะรองรับอย่างเป็นทางการสำหรับ 2 เวอร์ชันล่าสุดของเบราว์เซอร์หลัก ๆ

แม้ว่าเบราว์เซอร์รุ่นอื่นนอกเหนือจากช่วงนี้อาจทำงานได้ แต่เราอาจจะไม่ตรวจสอบหรือแก้ไขปัญหาในเบราว์เซอร์ที่ล้าสมัยมากนัก
หากคุณมีข้อเสนอแนะเล็ก ๆ น้อย ๆ ที่ช่วยขยายช่วงการรองรับโดยไม่กระทบกับเบราว์เซอร์ที่รองรับอยู่ เรายินดีรับพิจารณา

## ข้อตกลงการให้บริการ (Terms of Service)

`@vis.gl/react-google-maps` ใช้บริการของ Google Maps Platform การใช้บริการดังกล่าวผ่านไลบรารีนี้
ผูกพันตาม [Google Maps Platform Terms of Service][gmp-tos]

ไลบรารีนี้ไม่ใช่บริการหลักของ Google Maps Platform ดังนั้นเงื่อนไขบางอย่างของ Google Maps Platform (เช่น Technical Support Services, SLA และ Deprecation Policy)
อาจไม่ถูกนำมาบังคับกับไลบรารีนี้โดยตรง

### สำหรับผู้พัฒนาที่อยู่ในเขตเศรษฐกิจยุโรป (EEA)

หากที่อยู่สำหรับการเรียกเก็บเงินของคุณอยู่ในเขต EEA ตั้งแต่วันที่ 8 กรกฎาคม 2025 ข้อกำหนด [Google Maps Platform EEA Terms of Service][gmp-tos-eea]
จะมีผลกับการใช้บริการของคุณ การทำงานบางอย่างอาจแตกต่างกันไปตามภูมิภาค
[อ่านเพิ่มเติม][gmp-tos-eea-faq]

## การช่วยเหลือและการสนับสนุน

ไลบรารีนี้เป็นโอเพนซอร์ส หากพบบั๊กหรือต้องการฟีเจอร์ใหม่ กรุณา [เปิด issue][rgm-issues] บน GitHub
หากต้องการถามคำถามเชิงเทคนิคจากชุมชนนักพัฒนา Google Maps Platform คุณสามารถเปิดกระทู้ใน [GitHub Discussions][rgm-discuss]
หรือติดต่อผ่านช่องทางชุมชนนักพัฒนาต่าง ๆ [developer community channels][gmp-community]

หากต้องการร่วมพัฒนา โปรดดู [Contributing guide][rgm-contrib]

นอกจากนี้ยังมีช่องทางพูดคุยบน [Discord ของเรา][gmp-discord]

[api-provider]: https://visgl.github.io/react-google-maps/docs/api-reference/components/api-provider
[api-map]: https://visgl.github.io/react-google-maps/docs/api-reference/components/map
[api-marker]: https://visgl.github.io/react-google-maps/docs/api-reference/components/marker
[api-adv-marker]: https://visgl.github.io/react-google-maps/docs/api-reference/components/advanced-marker
[api-infowindow]: https://visgl.github.io/react-google-maps/docs/api-reference/components/info-window
[api-use-lib]: https://visgl.github.io/react-google-maps/docs/api-reference/hooks/use-maps-library
[docs]: https://visgl.github.io/react-google-maps/docs/
[examples]: https://visgl.github.io/react-google-maps/examples
[gmp-services]: https://developers.google.com/maps/documentation/javascript#services
[gmp-libraries]: https://developers.google.com/maps/documentation/javascript/libraries
[npm-package]: https://www.npmjs.com/package/@vis.gl/react-google-maps
[gmp-tos]: https://cloud.google.com/maps-platform/terms
[gmp-tos-eea]: https://cloud.google.com/terms/maps-platform/eea
[gmp-tos-eea-faq]: https://developers.google.com/maps/comms/eea/faq
[gmp-tssg]: https://cloud.google.com/maps-platform/terms/tssg
[gmp-sla]: https://cloud.google.com/maps-platform/terms/sla
[gmp-dp]: https://cloud.google.com/maps-platform/terms/other/deprecation-policy
[rgm-issues]: https://github.com/visgl/react-google-maps/issues
[rgm-discuss]: https://github.com/visgl/react-google-maps/discussions
[rgm-contrib]: https://visgl.github.io/react-google-maps/docs/contributing
[gmp-community]: https://developers.google.com/maps/developer-community
[gmp-discord]: https://discord.gg/f4hvx8Rp2q
[gmp-browsersupport]: https://developers.google.com/maps/documentation/javascript/browsersupport
