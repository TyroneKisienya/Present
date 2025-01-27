class CakeDecorator {
  constructor() {
    this.cakes = [
      { size: 0.5, frostingColor: '#8B4513' },
      { size: 0.75, frostingColor: '#FFA07A' },
      { size: 1, frostingColor: '#FFD700' }
    ];
    this.candle = {
      flame: 100,
      blowCount: 0
    };
    this.intervalId = null;
  }

  startFlickering() {
    this.intervalId = setInterval(() => {
      this.candle.flame = Math.max(0, this.candle.flame - Math.floor(Math.random() * 10));
      this.updateCandle();
    }, 100);
  }

  stopFlickering() {
    clearInterval(this.intervalId);
  }

  blowCandle() {
    this.candle.blowCount++;
    if (this.candle.blowCount === 1) {
      this.startFlickering();
    } else if (this.candle.blowCount === 2) {
      this.candle.flame = Math.max(0, this.candle.flame - 50);
      this.updateCandle();
    } else if (this.candle.blowCount >= 3) {
      this.candle.flame = 0;
      this.updateCandle();
      this.stopFlickering();
    }
  }

  updateCandle() {
    const candleElement = document.querySelector('.candle');
    candleElement.style.height = `${this.candle.flame}px`;
  }

  render() {
    const containerElement = document.createElement('div');
    containerElement.classList.add('cake-container');

    const candleElement = document.createElement('div');
    candleElement.classList.add('candle');
    candleElement.style.height = `${this.candle.flame}px`;
    candleElement.style.width = '20px';
    candleElement.style.backgroundColor = 'yellow';
    candleElement.style.position = 'relative';
    candleElement.style.top = '20px';
    candleElement.addEventListener('click', () => this.blowCandle());
    containerElement.appendChild(candleElement);

    this.cakes.forEach((cake, index) => {
      const cakeElement = document.createElement('div');
      cakeElement.classList.add('cake-layer');
      cakeElement.style.height = `${cake.size * 100}px`;
      cakeElement.style.width = `${cake.size * 200}px`;
      cakeElement.style.backgroundColor = cake.frostingColor;
      cakeElement.style.order = 3 - index;
      containerElement.appendChild(cakeElement);
    });

    return containerElement;
  }
}

const cakeDecorator = new CakeDecorator();
document.body.appendChild(cakeDecorator.render());