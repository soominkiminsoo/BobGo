
### Selinium으로 동적 웹페이지 소스코드 가져오기 (node-js)

1. npm 으로 selinium 설치하기

    
    [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver)
    
    ```bash
    npm install selenium-webdriver
    ```
    
2. 원하는 웹 브라우저 드라이버 설치하기
    
    위 페이지에 들어가서, 현재 컴퓨터에 설치되어 있는 버전과 같은 버전으로 다운 받는다.
    
    ![스크린샷 2022-09-09 오후 4.06.11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80ca8de5-c424-41da-8282-dc50a32c8c0c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-09-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.06.11.png)
    
    다운 받은 파일은 현 프로젝트 파일에 위치 시킨다.
    
3. 코딩
    
    ```jsx
    // selenium import
    const {Builder, Browser, By, until} = require('selenium-webdriver');
    
    // 밑 코드는 해당 url로 post 요청이 올 시 실행된다.
    // urls array 를 받아온다.
    app.post('/processing_timetable', async function(req, res) {
        var post = req.body;
        var urls = post.urls;
        var htmlbody;
    		
    		// Chrome browser driver 생성. Webdrive class
        let driver = await new Builder().forBrowser(Browser.CHROME).build();
        try {
    				// 생성한 driver로 url 실행
            await driver.get(urls[0]);
    				// 1000 ms 기다린 후, 'body'라는 css 선택자를 통해 element를 가져온다.
    				// document.body 와 비슷.
            const pageSource = await driver.wait(until.elementLocated(By.css('body')), 1000).getAttribute('innerHTML');
            htmlbody = pageSource;
    				// 응답 보내기
            res.send({result: htmlbody});
            console.log('pageSource: ', pageSource);
        } finally {
            await driver.quit();
        }
    
    })
    ```
