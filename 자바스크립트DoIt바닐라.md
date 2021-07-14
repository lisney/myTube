## Continue 홀수만 출력
```
    <script>
        for(let i=1;i<=10;++i){
            if(i%2==0)continue
            document.write(i+'<br>')
        }
        document.querySelector('section').write('=== End Game ===')
    </script>
```

## while 구구단
![image](https://user-images.githubusercontent.com/30430227/125612130-89e1ec7a-7f3b-4d7c-a1c8-9476bde807aa.png)
```
    <script>
        let i = 1
        while(i<=9){
            document.write(`5 X ${i} = ${i*5} <br>`)
            i+=1
        }
    </script>
```

## for 테이블
![image](https://user-images.githubusercontent.com/30430227/125613342-32a45c14-499e-4329-9390-c43bacde258f.png)
```
    <script>
        let num = 1
        let t = '<table border=1>'

        for(let i=0;i<5;++i){
            t += '<tr>'
                for(let j=0;j<5;++j){
                    t += `<td> ${num} </td>`
                    num++
                }
            t += '</tr>'
        }

        t += '</table>'
        document.write(t)
    </script>
```

