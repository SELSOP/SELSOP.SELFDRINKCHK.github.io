// 캐시 이름 정의
const CACHE_NAME = 'bac-checker-cache-v1';
// 캐시할 파일 목록
const urlsToCache = [
  '/',
  'index.html' // 앱의 메인 HTML 파일 이름
];

// 1. 서비스 워커 설치
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => {
        console.log('Opened cache');
        return cache.addAll(urlsToCache);
      })
  );
});

// 2. 요청에 응답 (캐시된 파일 우선 사용)
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        // 캐시에 파일이 있으면 그것을 반환, 없으면 네트워크에서 가져옴
        if (response) {
          return response;
        }
        return fetch(event.request);
      }
    )
  );
});
