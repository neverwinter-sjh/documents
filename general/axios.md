# Axios 설정

## axios.js

axios의 기본 setting 파일을 설정한다.

```
import axios from 'axios';

/**
 * axios 인스턴스를 생성
 * 옵션은 https://github.com/axios/axios 의 Request Config 챕터 확인
 * @constructor
 * @param {string} baseUrl - 기본 주소
 * @param {number} timeout - 통신 최대 대기 시간. 설정값을 넘게 되면 통신을 멈추고 에러를 반환.
 */
const instance = axios.create({
  baseURL: 'http://api.openweathermap.org/data/2.5',
  timeout: 10000,
});

/**
 * Request 직전 설정
 * @function
 * @param {function} anonymous - 요청 성공 직전 호출되는 함수. axios 설정값을 넣는다. (사용자 정의 설정도 추가 가능)
 * @param {function} anonymous - 요청 에러 직전 호출되는 함수. 요청 관련 error argument를 받음
 */
instance.interceptors.request.use(
  function (config) {
    // try {
    //   console.group('Axios Request 시작');
    //   console.log(config);
    //   console.groupEnd();
    // } catch (e) {
    //   console.log(e);
    // }
    return config;
  },
  function (error) {
    // try {
    //   console.group('Axios Request 에러');
    //   console.log(error);
    //   console.groupEnd();
    // } catch (e) {
    //   console.log(e);
    // }
    return Promise.reject(error);
  }
);

/**
 * Response 직후 설정
 * @function
 * @param {function} anonymous - http status 200. 응답 성공 직전 호출. then()으로 이어짐.
 * @param {function} anonymous - http 200이 아닌 경우. 응답 에러 직전 호출. catch()로 이어짐
 */
instance.interceptors.response.use(
  function (response) {
    // try {
    //   console.group('Axios Response 성공');
    //   console.log(response);
    //   console.groupEnd();
    // } catch (e) {
    //   console.log(e);
    // }
    return response;
  },
  function (error) {
    // try {
    //   console.group('Axios Response 실패');
    //   console.log(error);
    //   console.groupEnd();
    // } catch (e) {
    //   console.log(e);
    // }
    return Promise.reject(error);
  }
);

export default instance;

```
