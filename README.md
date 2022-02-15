# Timer Class
```javascript
class Timer {

    Timer() {
        this.time = 0;
        this.element = null;
        this.control = true;
        this.callback = null;
        this.timeLimit = 10;
    }

    set(time, id, callback = null) {
        this.time = time;
        this.element = document.getElementById(id);
        this.callback = callback;
    }

    setTimeLimit(time) {
        this.timeLimit = time;
    }

    start(type = 'COUNT_DOWN') {
        this.control = true;

        setTimeout(() => {
            if(type == 'COUNT_DOWN')
                this.countDown();
            else if(type == 'COUNT_UP') 
                this.countUp();
        }, 1000);
    }

    format() {
        let hours = parseInt(this.time / 3600);
        let timeLeft = this.time - hours * 3600;
        let minutes = parseInt(timeLeft / 60);
        timeLeft = timeLeft - minutes * 60;
        let seconds = timeLeft;
        
        hours = hours.toString();
        minutes = minutes.toString();
        seconds = seconds.toString();
    
        if (hours.length == 1)
            hours = '0' + hours;
        if (minutes.length == 1)
            minutes = '0' + minutes;
        if (seconds.length == 1)
            seconds = '0' + seconds;
        
        return hours + ':' + minutes + ':' + seconds;
    }

    countDown() {
        if(!this.control)
            return;
        let timerblock = this.element;
        timerblock.innerHTML = this.format();
        timerblock.style.display = 'block';

        if (this.time <= 59) {
            timerblock.style.color = 'red';
        }
    
        if (this.time <= 0) {
            timerblock.innerHTML = 'Time end!';
            this.callback();
            this.stop();
        }
        else {
            setTimeout(() => {
                this.countDown();
            }, 1000);
            this.time--;
        }
    }

    countUp() {
        if(!this.control)
            return;
        let timerblock = this.element;
        timerblock.innerHTML = this.format();
        timerblock.style.display = 'block';
    
        if(this.time >= this.timeLimit) {
            timerblock.innerHTML = 'Timer Limit Exceed!';
            this.callback();
            this.stop();
        }
        else {
            setTimeout(() => {
                this.countUp();
            }, 1000);
            this.time++;
        }
    }

    stop() {
        this.control = false;
    }
}
```
### Create a div set attribute id='timer'
```html
<div id="timer"></div>
```

#### Create a callback function which you want to call after
1. time == 0 in COUNT_DOWN Timer
2. time limit exceeds in COUNT_UP Timer
```javascript
const callback = () => {
    console.log('callback function called from timer');
}
```

### Create timer instance
```javascript
const timer = new Timer();
```

### set method take 3 arguments
1. time
2. dom element id
3. callback function
```javascript
timer.set(10, 'timer', callback);
```

# Finally call start function to start the timer
It takes a string 'COUNT_DOWN' or 'COUNT_UP' by default it is 'COUNT_DOWN'
```javascript
timer.start('COUNT_DOWN');
```

# To stop timer
```javascript
timer.stop();
```


