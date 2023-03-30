// Define the Square component
@Component
class Square extends Vue {
  @Prop({ type: Number, required: true }) size!: number;
  @Prop({ type: Number, required: true }) index!: number;
  @Prop({ type: Boolean, default: false }) occupied!: boolean;
  @Prop({ type: Boolean, default: false }) blocked!: boolean;

  get textStyle() {
    return {
      position: 'absolute',
      top: '50%',
      left: '50%',
      transform: 'translate(-50%, -50%)',
    };
  }

  render() {
    return (
      <div
        style={{
          position: 'relative',
          width: `${this.size}px`,
          height: `${this.size}px`,
          backgroundColor: this.blocked ? 'gray' : this.occupied ? 'blue' : 'white',
        }}
      >
        <span style={this.textStyle}>{this.index}</span>
      </div>
    );
  }
}

// Define the Grid component
@Component
class Grid extends Vue {
  @Prop({ type: Number, required: true }) n!: number;

  get squareSize() {
    return 50; // Change this to whatever size you want
  }

  render() {
    return (
      <div style={{ display: 'flex', flexWrap: 'wrap' }}>
        {Array.from({ length: this.n * this.n }, (_, i) => {
          const x = i % this.n;
          const y = Math.floor(i / this.n);
          return (
            <Square
              key={i}
              size={this.squareSize}
              index={i + 1}
              blocked={x === 0 || y === 0 || x === this.n - 1 || y === this.n - 1}
            />
          );
        })}
      </div>
    );
  }
}
