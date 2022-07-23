<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="file.css" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.min.css"
    />
  </head>
  <body>
    <div class="timer">
      <div class="controls">
        <input id="duration" value="3" />
        <div>
          <button id="start"><i class="fas fa-play"></i></button>
          <button id="pause"><i class="fas fa-pause"></i></button>
        </div>
      </div>
      <svg class="dial">
        <circle
          fill="transparent"
          stroke="green"
          stroke-width="15"
          r="190"
          cx="0"
          cy="200"
          transform="rotate(-90 100 100)"
        />
      </svg>
    </div>

    <script src="timer.js"></script>
    <script src="app.js"></script>
  </body>
</html>


const durationInput = document.querySelector('#duration');
const startButton = document.querySelector('#start');
const pauseButton = document.querySelector('#pause');
const circle = document.querySelector('circle');

const perimeter = circle.getAttribute('r') * 2 * Math.PI;
circle.setAttribute('stroke-dasharray', perimeter);

let duration;
const timer = new Timer(durationInput, startButton, pauseButton, {
    onStart(totalDuration) {
      duration = totalDuration;
    }, 
    onTick(timeRemaining) {
        circle.setAttribute('stroke-dashoffset',
        perimeter * timeRemaining / duration - perimeter
        );
    },
    onComplete() {
        console.log('Timer is completed');
    }
});


class Timer {
    constructor(durationInput, startButton, pauseButton, callbacks) {
        this.durationInput = durationInput;
        this.startButton = startButton;
        this.pauseButton = pauseButton;
        if (callbacks) {
          this.onStart = callbacks.onStart;
          this.onTick = callbacks.onTick;
          this.onComplete = callbacks.onComplete;
        }

        this.startButton.addEventListener('click', this.start);
        this.pauseButton.addEventListener('click', this.pause);
    }

    start = () => {
        if(this.onStart){
            this.onStart(this.timeRemaining);
        }
        this.tick();
        this.interval = setInterval(this.tick, 50);
    };

    pause = () => {
      clearInterval(this.interval);
    };
       
    tick = () => {
      if (this.timeRemaining <= 0) {
        this.pause();
        if (this.onComplete) {
            this.onComplete()
        };
      } else {
        this.timeRemaining = this.timeRemaining - 0.5;
        if (this.onTick) {
            this.onTick(this.timeRemaining);
        }
      } 
    };

    get timeRemaining() {
        return parseFloat(this.durationInput.value);
    }

    set timeRemaining(time) {
        this.durationInput.value = time.toFixed(2);
    }
}

.dial {
    height: 400px;
    width: 400px;
  }
  
  .timer {
    position: relative;
    display: inline-block;
  }
  
  * {
    font: inherit;
  }
  
  body {
    font-family: 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Fira Sans',
      'Droid Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
  }
  
  .timer input {
    display: block;
    border: none;
    width: 240px;
    font-size: 90px;
    text-align: center;
  }
  
  .timer button {
    border: none;
    font-size: 36px;
    cursor: pointer;
  }
  
  .timer button:focus {
    outline: none;
  }
  
  .timer input:focus {
    outline: none;
  }
  
  .controls {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
