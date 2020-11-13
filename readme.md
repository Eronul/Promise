一个手写的实现简易 Promise 的页面
代码：
class Promise2 {
  state = 'pending'
  succeed = null
  fail = null

    resolve(result) {      
      setTimeout(() => {
        this.state = 'fulfilled'
        this.succeed(result)
      })
    }
    
    reject(err) {
      setTimeout(() => {
        this.state = 'rejected'
        this.fail(err)
      })
    }
    
    constructor(fn) {
      fn(this.resolve, this.reject)
    }

    then(succeed,fail) {
      this.succeed = succeed
      this.fail = fail
    }
  }
  
  
  const getWeather = city => new Promise2((resolve, reject) => {
    let xhr = new XMLHttpRequest()
    xhr.open('GET',
  'http://rap2.taobao.org:38080/app/mock/245421/getWeather?city='+city, true)
    xhr.onload = () => {
      if(xhr.status === 200) {
        resolve(JSON.parse(xhr.responseText))
      } else {
        reject(`获取${city}天气失败`)
      }
    }
    xhr.send()
  })
  
  getWeather('北京')
  .then(data => {
    console.log(data)
  }, err => {
    console.log(err)
  })
