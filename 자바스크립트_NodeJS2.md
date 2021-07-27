## 주소록

![image](https://user-images.githubusercontent.com/30430227/127125838-8c2ae4a8-0989-4b82-ab8d-ff4a9507d716.png)
```
const express = require('express')
const hbs = require('express-handlebars')
const mongoose = require('mongoose')
const bodyParser = require('body-parser')

require('dotenv').config({path:'variables.env'})

const {Schema} = mongoose //mongoose에서 Schema 객체만 변수에 연결

const server = express()

server.engine('hbs', hbs({
    extname:'hbs',
    defaultLayout:'layout',
    layoutsDir:__dirname+'/views/layouts',
    partialsDirs:__dirname+'/views/partials'
}))

server.set('view engine','hbs')
server.use(express.static(__dirname+'/static'))
server.use(bodyParser.json())
server.use(bodyParser.urlencoded({extended:true}))  //form 입력 데이터가 req.body로 생성

//DB setting 기본세팅 및 연결
mongoose.set('useNewUrlParser', true)
mongoose.set('useFindAndModify',false)
mongoose.set('useCreateIndex', true)
mongoose.set('useUnifiedTopology', true)
mongoose.connect(process.env.MONGODB_URL)

const db = mongoose.connection
db.once('open', ()=>{
    console.log('DB connected')
})
db.on('error', err=>{
    console.log('DB ERROR:', err)
})

//DB Schema
const contactSchema = new Schema({
    name:{type:String, required:true, unique:true}, // unique 값이 중복안됨
    email:{type:String},
    phone:{type:String}
})

const Contact = mongoose.model('contact', contactSchema) // mongoose.model  contactSchema 모델 생성, contact 콜렉션 이름, 콜렉션을 Contact변수에 연결

//Routes
server.get('/', (req, res)=>{
    res.redirect('/contacts') //응답을 다른 라우터로 보냄
})


server.get('/contacts',(req, res)=>{
    Contact.find({},(err, contacts)=>{ //{} (=검색조건 없음) DB에서 해당 모델의 모든 data를 리턴
        if(err)return res.json(err)
        res.render('contacts/index',{contacts:contacts})
    })
})

server.get('/contacts/new',(req, res)=>{
    res.render('contacts/new')
})

server.post('/contacts',(req, res)=>{
    Contact.create(req.body, (err, contact)=>{
        if(err) return res.json(err)
        res.redirect('/contacts')
    })
})

server.listen(3000, ()=>{
    console.log('The server is running...')
})
```

![image](https://user-images.githubusercontent.com/30430227/127126797-f0853a8d-87e0-4f98-9b61-cdd6d0a44121.png)
```
<h2>Index</h2>
<ul>
    {{#each contacts}}
    <li>{{#each this}}{{name}}{{/each}}</li>
    {{/each}}
</ul>
```
```
<h2>New</h2>
<form action="/contacts" method="post">
    <div>
    <label for="name">Name</label>
    <input type="text" id="name" name="name">
    </div>
    <div>
    <label for="email">Email</label>
    <input type="text" id="email" name="email">
    </div>
    <div>
    <label for="phone">Phone</label>
    <input type="text" id="phone" name="phone">
    </div>

    <input type="submit" value="등록">
</form>
```

