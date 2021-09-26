##### Node.js와 Firestore 연동


<blockquote data-ke-style="style2"><br /><b>1. Firebase 모듈 설치</b></blockquote>
<p data-ke-size="size16">&nbsp;시작에 앞서 우리는 Firebase 모듈을 설치해야한다. 이전에 우리는 <a href="https://alkorithm.tistory.com/entry/Nodejs-Theory-%EB%85%B8%EB%93%9Cnode-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-windows-10%EB%B2%84%EC%A0%84-ALKORITHM?category=495391" target="_blank" rel="noopener">Node를 설치하였고, 명령 프롬프트(cmd) 창에서 node를 자유롭게 이용할 수 있게 되었다</a>.(설치하지 않았다면 링크를 눌러서 설치해보자...!) 따라서 우리는 아래의 명령어를 cmd 창에서 입력해주면 된다.</p>
<pre id="code_1630822360995" class="shell" data-ke-language="shell" data-ke-type="codeblock"><code>npm install firebase-admin --save</code></pre>
<p data-ke-size="size16">&nbsp;해당 명령어를 치면 알아서 Firebase-admin 모듈이 설치된다.</p>
<pre id="code_1632411928779" class="shell" data-ke-language="shell" data-ke-type="codeblock"><code>npm install body-parser
npm install cors
npm install dotenv
npm install express
npm install body-parser</code></pre>
<p data-ke-size="size16">&nbsp;위의 모듈도 추가적으로 설치해주자.</p>
<blockquote data-ke-style="style2"><br /><b>2. Firebase 프로젝트에 웹 추가 및 추가 작업</b></blockquote>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134799867-a08012f9-2e9f-4ae9-9775-f668914488f3.png"></p> 
<p data-ke-size="size16">&nbsp;Firebase에 접속하면 이렇게 프로젝트를 선택할 수 있다. (처음 사용하는 사람을 없을 거라 생각하고 작성하겠다.) 여기서 원하는 프로젝트를 선택하면 된다.&nbsp;</p>
<p style="text-align: left;" data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134799911-5c02a656-c60c-4d03-97fc-c20343eb9924.png"  ></p> 
<p style="text-align: left;" data-ke-size="size16">&nbsp;선택하여 들어가서 <b>앱 추가</b>를 눌러준다.</p>
<p style="text-align: left;" data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134799947-a09c1796-3780-4438-8cfa-3521958575d9.png" ></p> 
<p style="text-align: left;" data-ke-size="size16">우리는 오늘 Node js와 연동하기에 웹 버튼을 눌러 연동해준다. 이 과정에서 Firebase SDK를 확인할 수 있는데, 이 부분은 이후에도 설정에서 확인할 수 있고, Firestore와의 연동에서는 사용하지 않으니 일단 넘어가도록 하자.</p>
<p style="text-align: left;" data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800080-31e0e637-269b-4645-9c53-fff50df2efad.png" ></p> 
<p data-ke-size="size16">&nbsp;Firestore 연동에서 가장 중요한 부분이다. <b>[프로젝트 설정] &rarr; [서비스 계정] &rarr; [서비스 계정 만들기]</b> 를 통해 Firebase Admin SDK가 담긴 JSON 파일을 다운 받아준다.&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800120-5c7cecf6-341c-46f5-947a-ba8d547a78b2.png" ></p> 
<p data-ke-size="size16">&nbsp;그리고 오늘의 메인인 Firestore 데이터베이스를 만들어준다. 만드는 과정에서 설정은 <b>테스트 모드</b>와 <span style="color: #000000;"><b>asia-northeast3</b>로 설정해주었다.&nbsp;</span></p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><span style="color: #000000;">&nbsp;그럼 지금부터 Node 코드를 살펴보자.</span></p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>3. Node Project 소개</b></blockquote>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800163-e008e51d-debd-47be-aa17-c2a05d7e2050.png" ></p> 
<p data-ke-size="size16">&nbsp;위는 디렉토리 안에 있는 하위 디렉토리 및 파일들이다. 간단하게 설명한 후 하나씩 코드를 살펴보자.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;먼저 controllers 디렉토리 안에 있는 <b>dataController.js</b> 파일의 코드에는 웹 통신을 위한 POST, GET, PUT 등의 메서드와 오늘의 메인인 Firebase가 담겨있다. 사실상 오늘의 메인이라고 할 수 있는 파일이다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;models 디렉토리의 <b>Table.js</b> 파일은 Firestore DB에 쓸 데이터 양식이 클래스 형식(또는 프로토타입 형식)으로 담겨있다. 그 밑의 <b>node_modules </b>디렉토리는 우리가 위에서 npm install했던 Node의 모듈들이 저장된 디렉토리다. path 디렉토리의 <b>serviceAccountKey.json</b> 파일은 위에서 다운 받은 Firebase Admin SDK가 담긴 json 파일이다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;routes 디렉토리의<b> routes.js</b>는 말 그대로 router가 담겨있는 파일로, express에 대해 공부했다면 다들 알거라 생각이 들어 자세히 설명하진 않겠다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;나머지 파일을 하나씩 설명하면 <b>.env</b>는 <span style="color: #202124;">특정 process를 위한 key-value 형태의 환경변수들이 담긴 파일로 보안을 위해 생성해 놓았다.(굳이 필요하진 않지만, 사용한다면 프로젝트의 완성도가 상승한다...!) <b>config.js</b> 파일은 .env의 환경변수들을 받아와 모듈화 시켜주는 파일이다. 이 밖에 <b>index.js</b>는 node를 실행했을 때 메인이 되는 파일로, listen을 실행한 파일이고, <b>package.json</b>은 <span style="color: #555555;">현재 프로젝트에 대한 정보를 저장하는 곳으로, npm init을 통해 생성할 수 있고, 이 부분도 기본 지식이므로 넘어가도록 하겠다.</span></span></p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><span style="color: #202124;"><span style="color: #555555;">&nbsp;그럼 지금부터 본격적으로 코드를 살펴볼 것이다.</span></span></p>
<p data-ke-size="size16"><span style="color: #202124;"><span style="color: #555555;"> 코드는 [<b>Table.js</b>]&nbsp;&rarr;&nbsp;[<b>.env</b>]&nbsp;&rarr;&nbsp;[<b>config.js</b>]&nbsp;&rarr;&nbsp;[<b>index.js</b>]&nbsp;&rarr;&nbsp;[<b>routes.js</b>]&nbsp;&rarr;&nbsp;[<b>dataController.js</b>]&nbsp;순서대로&nbsp;볼&nbsp;것이다.</span></span></p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>4.</b> <b>Table.js&nbsp;</b></blockquote>
<pre id="code_1632641937276" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>class Table {
    constructor(userId, useInfo, useTable) {
            this.userId = userId;
            this.useInfo = useInfo;
            this.useTable = useTable;
    }
}

module.exports = Table;</code></pre>
<p data-ke-size="size16">&nbsp;딱히 설명할 부분 없는 코드다. Firestore DB로 관리할 데이터 양식이라고 생각하면 된다. 원하는 데이터 양식으로 수정해도 되고, 마지막에 module.exports를 통해 모듈로 만들어준다.</p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>5.</b> <b>.env</b>&nbsp;</blockquote>
<pre id="code_1632642113912" class="shell" data-ke-language="shell" data-ke-type="codeblock"><code>NODE_ENV=production
#express server config

PORT=8080
HOST=localhost
HOST_URL=http://localhost:8080</code></pre>
<p data-ke-size="size16">&nbsp; 이 코드 또한 딱히 설명할 부분은 없다. PORT 번호를 저장해 놓았고, 만약 Firebase 연동이 필요하다면 API KEY를 기재해 놓아도 된다. (이번 실습에서는 API KEY를 사용하지 않으므로 제외해 두었다.)</p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>6. config.js</b></blockquote>
<pre id="code_1632642293277" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>'use strict';
const dotenv = require('dotenv');
const assert = require('assert');

dotenv.config();

const {
&nbsp;    PORT,
&nbsp;    HOST,
&nbsp;    HOST_URL
} = process.env;

assert(PORT, 'PORT is required');
assert(HOST, 'HOST is required');

module.exports = {
&nbsp;    port: PORT,
&nbsp;    host: HOST,
&nbsp;    url: HOST_URL,
}</code></pre>
<p data-ke-size="size16">&nbsp;dotenv 모듈을 사용해서 .env를 활용해주었다. PORT, HOST, HOST_URL을 .env 파일에서 가져와 모듈화 해주었다.</p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>7. index.js</b></blockquote>
<pre id="code_1632642382725" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>'use strict';

const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');
const config = require('./config');
const routes = require('./routes/routes');

const app = express();

app.use(express.json());
app.use(cors());
app.use(bodyParser.json());

app.use('/api', routes.routes);

app.listen(config.port, () =&gt; console.log('App is listening on url http://localhost:' + config.port));</code></pre>
<p data-ke-size="size16">&nbsp; app 변수에 express() 모듈을 넣어주고, use( ) 메서드를 통해 install했던 모듈들을 받아준다. 이후 listen을 통해 서버를 열어준다.</p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br />8. routes.js</blockquote>
<pre id="code_1632642580489" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>const express = require('express');
const {
    addTable, 
    getAllTableInfo, 
    getTable,
    updateTable,
    deleteTable
} = require('../controllers/dataController');

const router = express.Router();

router.post('/table', addTable);
router.get('/Table_Use_Information', getAllTableInfo);
router.get('/table/:id', getTable);
router.put('/table/:id', updateTable);
router.delete('/table/:id', deleteTable);

module.exports = {
&nbsp;    routes: router
}</code></pre>
<p data-ke-size="size16">&nbsp;읽어보기만 해도 알 것 같은 코드다. dataController.js 파일에서 모듈을 받아와 POST, GET, PUT, DELETE 등의 작업을 router에 정의하여 모듈화 하였다. 그럼 이제 메인인 dataController.js 파일을 살펴보자.</p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>9. dataControllers.js</b></blockquote>
<pre id="code_1632642696646" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>'use strict';

const Table = require('../models/Table');
const admin = require('firebase-admin');
var serviceAccount = require("../path/serviceAccountKey.json");

admin.initializeApp({
&nbsp;  credential: admin.credential.cert(serviceAccount)
});

const db = admin.firestore();

const addTable = async (req, res, next) =&gt; {
&nbsp;    try {
&nbsp;        const data = req.body;
&nbsp;        await db.collection('Table_Use_Information').doc().set(data);
&nbsp;        res.send('Record saved successfuly');
&nbsp;        console.log('Record saved successfuly');
&nbsp;    } catch(error) {
&nbsp;        res.status(400).send(error.message);
&nbsp;        console.log("Failed with error: " + error);
&nbsp;    }
}

const getAllTableInfo = async (req, res, next) =&gt; {
&nbsp;    try {
&nbsp;        const tableInfo = await db.collection('Table_Use_Information');
&nbsp;        const data = await tableInfo.get();
&nbsp;        const tableInfoArray = [];
&nbsp;        if(data.empty) {
&nbsp;            res.status(404).send('No Table record found');
&nbsp;        }else {
&nbsp;            data.forEach(doc =&gt; {
&nbsp;                const table = new Table(
&nbsp;                    doc.id,
&nbsp;                    doc.data().firstName,
&nbsp;                    doc.data().lastName,
&nbsp;                    doc.data().fatherName,
&nbsp;                    doc.data().class,
&nbsp;                    doc.data().age,
&nbsp;                    doc.data().phoneNumber,
&nbsp;                    doc.data().subject,
&nbsp;                    doc.data().year,
&nbsp;                    doc.data().semester,
&nbsp;                    doc.data().status
&nbsp;                );
&nbsp;                tableInfoArray.push(table);
&nbsp;            });
&nbsp;            res.send(tableInfoArray);
&nbsp;        }
&nbsp;    } catch (error) {
&nbsp;        res.status(400).send(error.message);
&nbsp;    }
}

const getTable = async (req, res, next) =&gt; {
&nbsp;    try {
&nbsp;        const id = req.params.id;
&nbsp;        const table = await db.collection('Table_Use_Information').doc(id);
&nbsp;        const data = await table.get();
&nbsp;        if(!data.exists) {
&nbsp;            res.status(404).send('Table with the given ID not found');
&nbsp;        }else {
&nbsp;            res.send(data.data());
&nbsp;        }
&nbsp;    } catch (error) {
&nbsp;        res.status(400).send(error.message);
&nbsp;    }
}

const updateTable = async (req, res, next) =&gt; {
&nbsp;    try {
&nbsp;        const id = req.params.id;
&nbsp;        const data = req.body;
&nbsp;        const table =  await db.collection('Table_Use_Information').doc(id);
&nbsp;        await table.update(data);
&nbsp;        res.send('Table record updated successfuly');        
&nbsp;    } catch (error) {
&nbsp;        res.status(400).send(error.message);
&nbsp;    }
}

const deleteTable = async (req, res, next) =&gt; {
&nbsp;    try {
&nbsp;        const id = req.params.id;
&nbsp;        await db.collection('Table_Use_Information').doc(id).delete();
&nbsp;        res.send('Record deleted successfuly');
&nbsp;    } catch (error) {
&nbsp;        res.status(400).send(error.message);
&nbsp;    }
}

module.exports = {
&nbsp;    addTable,
&nbsp;    getAllTableInfo,
&nbsp;    getTable,
&nbsp;    updateTable,
&nbsp;    deleteTable
}</code></pre>
<p data-ke-size="size16">&nbsp;복붙을 하시는 분들을 위해 전체 코드를 올렸는데, 설명하기 힘들어 몇몇 부분만 뜯어서 설명하겠다.</p>
<p data-ke-size="size16">&nbsp;</p>
<pre id="code_1632642754770" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>const Table = require('../models/Table');
const admin = require('firebase-admin');
var serviceAccount = require("../path/serviceAccountKey.json");

admin.initializeApp({
&nbsp;  credential: admin.credential.cert(serviceAccount)
});

const db = admin.firestore();</code></pre>
<p data-ke-size="size16">&nbsp;첫 부분이다. 먼저 data 형식인 Table을 모듈로 받아 변수에 저장해주었다. 또 맨 처음에 npm install했던 firebase-admin 모듈을 admin 변수에 저장해주고, Firebase Admin SDK가 담긴 json 파일인 serviceAccount 또한 변수에 저장해준다.&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;이후 공식문서에서 설명돼 있듯이 initializeApp()을 통해 admin 변수를 초기화해주고, firestore( )를 통해 firestore를 사용해준다.</p>
<p data-ke-size="size16">&nbsp;</p>
<pre id="code_1632642913991" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>const addTable = async (req, res, next) =&gt; {
    try {
        const data = req.body;
        await db.collection('Table_Use_Information').doc().set(data);
        res.send('Record saved successfuly');
        console.log('Record saved successfuly');
    } catch(error) {
        res.status(400).send(error.message);
        console.log("Failed with error: " + error);
    }
}</code></pre>
<p data-ke-size="size16">&nbsp;많은 함수 중 이 함수를 살펴볼 것이다. 비동기적으로 실행되므로 async와 await를 통해 구현됐다. data에 user의 request 요청의 body를 넣어주고 firestore에 간단히 데이터를 전송해주었다.&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;이제 실행이 잘되는지 테스트해보자.</p>
<p data-ke-size="size16">&nbsp;</p>
<blockquote data-ke-style="style2"><br /><b>10. 실행</b></blockquote>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800307-68a5896e-41c4-4638-b629-05c7e65e4cc4.png" ></p> 
<p style="text-align: center;" data-ke-size="size16">먼저, npm start를 통해 웹 서버를 열어준다.</p>
<p style="text-align: center;" data-ke-size="size16">이제 POST를 실행해 볼 것인데, 필자는 VS Code에서 제공하는 Thunder Client를 이용하여 실습하였다.</p>
<p style="text-align: center;" data-ke-size="size16">(POSTMAN을 사용하든 다른 Gui를 사용하든 아무 상관 없다.)</p>
<p style="text-align: center;" data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800336-c99f5262-19b3-4674-8d08-bf02108ffd43.png" ></p> 
<p style="text-align: center;" data-ke-size="size16">현재 Firestore DB의 데이터이다. Thunder Client를 통해 Table_1을 추가해보자.</p>
<p style="text-align: center;" data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800367-bfe24b64-7549-4e0c-a984-8d28094aad15.png" ></p> 
<p style="text-align: center;" data-ke-size="size16">이렇게 데이터를 써주고 POST해주면 성공적으로 기록되었다고 터미널에 출력된다.</p>
<p style="text-align: center;" data-ke-size="size16">&nbsp;</p>
<p align="center"><img src="https://user-images.githubusercontent.com/56003992/134800385-b47566f0-b589-4685-96fd-e0f8c3936b00.png" ></p> 
<p style="text-align: center;" data-ke-size="size16">Firestore DB를 보면 정상적으로 들어간 것을 볼 수 있다.</p>
<p style="text-align: center;" data-ke-size="size16">다른 과정도 모두 비슷한데, routes를 이용한 만큼 작업을 할 때 url 적는 것을 주의하면 된다.</p>
<p style="text-align: center;" data-ke-size="size16">&nbsp;</p>
<hr contenteditable="false" data-ke-type="horizontalRule" data-ke-style="style5" />
<p style="text-align: left;" data-ke-size="size16"><b>참고 문서</b>: <a href="https://firebase.google.com/docs/firestore/quickstart?hl=ko#node.js" target="_blank" rel="noopener">Firebase 공식 문서</a></p>
<p data-ke-size="size16"><b>참고 YouTube</b>: <a href="https://www.youtube.com/watch?v=YPsftzOURLw" target="_blank" rel="noopener">Syed Ashraf</a></p>
<p data-ke-size="size16"><b>GitHub</b>: <a href="https://github.com/khsexk/Node.js-With-Firestore" target="_blank" rel="noopener"><b>khsexk Hub</b></a></p>
