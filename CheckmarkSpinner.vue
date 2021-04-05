<template>
  <div class="checkmark" :style="checkmarkCSSVars">
    <svg version="1.1" xml:space="preserve" x="0px" y="0px"
        viewBox="0, 0, 100, 100" id="checkmark">
      <g transform="">
        <circle 
          :class="['path', {'path-animate': isActive, 'path-complete': isFinishedChangeColor}]" 
          fill="none" 
          :stroke="currentColor" 
          stroke-width="4" 
          stroke-miterlimit="10" 
          cx="50" cy="50" r="44"/>
        <circle 
          :class="['fill', {'fill-animate': isActive, 'fill-complete': isFinishedLoading, 'change-color': isFinishedChangeColor}]" 
          fill="none" 
          :stroke="currentColor" 
          stroke-width="4" 
          stroke-miterlimit="10" 
          cx="50" cy="50" r="44"/>
        <polyline 
          :class="['check', {'check-animate': isActive, 'check-complete': isFinishedLoading, 'change-color': isFinishedChangeColor}]" 
          fill="none" 
          :stroke="currentColor" 
          stroke-width="8" 
          stroke-linecap="round" 
          stroke-miterlimit="10" 
          points="70,35 45,65 30,52"/>
      </g>
    </svg>
</div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from "vue-property-decorator";

@Component({
  components: {},
})
export default class CheckmarkSpinner extends Vue {
  @Prop({ default: "#7DB0D5" }) preColor!: string;
  @Prop({ default: "#15a115" }) postColor!: string;
  @Prop({ default: "100px" }) size!: string; 
  private CHANGE_TIMEOUT: number = 500; 
  private currentColor: string = this.preColor;
  private isActive: boolean = true;
  private isFinishedLoading: boolean = false;
  private isFinishedChangeColor: boolean = false;

  private mounted() {
    this.spin();
  }

  get checkmarkCSSVars() {
    return {
      '--width': this.size
    };
  }

  public spin() {
    this.isActive = false;
    setTimeout(() => {
      this.isFinishedLoading = false;
      this.currentColor = this.preColor;
      this.isFinishedChangeColor = false;
      this.isActive = true;
    });
  }

  public finishSpinning() {
    this.isFinishedLoading = true;

    setTimeout(() => {
      if (this.isFinishedLoading) {
        this.currentColor = this.postColor;
        this.isFinishedChangeColor = true;
      }
    }, this.CHANGE_TIMEOUT);
  }
}
</script>

<style scoped>
.checkmark {
  --width: 100px;
  width: var(--width);
  margin: 0 auto;
  padding-top: 40px;
}

.path {
  stroke-dasharray: 300;
  stroke-dashoffset: 0;
  -webkit-transform-origin: 50% 50%;
  -moz-transform-origin: 50px 50px;
}

.path-animate {
  -webkit-animation-name: load, spin;
  -webkit-animation-duration: 1.5s, 1.5s;
  -webkit-animation-timing-function: linear;
  -webkit-animation-iteration-count: infinite;
  animation-name: load, spin;
  animation-duration: 1.5s, 1.5s;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}

.path-complete {
    -webkit-animation-play-state: paused;
    animation-play-state: paused;
    stroke: rgba(0, 0, 0, 0);
}

.check 
{
	stroke-dasharray: 110;
	stroke-dashoffset: -110;
	stroke-width: 0;
}

.check-complete 
{   
	-webkit-animation: check 0.5s ease-in forwards;
	animation: check 0.5s ease-in forwards;
	stroke-width: 15;
	stroke-dashoffset: 0;
}

.fill 
{
	stroke-dasharray: 285;
	stroke-dashoffset: -257;
  -webkit-transform-origin: 50% 50%;
  -moz-transform-origin: 50px 50px;
}

.fill-animate {
  -webkit-animation: spin-fill 1.5s cubic-bezier(0.700, 0.435, 0.120, 0.600) infinite forwards;
	animation: spin-fill 1.5s cubic-bezier(0.700, 0.465, 0.120, 0.600) infinite forwards;
}

.fill-complete 
{
	-webkit-animation: fill 0.5s ease-out forwards;
	animation: fill 0.5s ease-out forwards;
}

@-webkit-keyframes load {
 0% {
   stroke-dashoffset: 300;
   -webkit-animation-timing-function: cubic-bezier(0.550, 0.055, 0.675, 0.190);
 }
 50% {
	 stroke-dashoffset: 0;
	 -webkit-animation-timing-function: cubic-bezier(0.215, 0.610, 0.355, 1.000);
 }
 100% {
   stroke-dashoffset: -300;
 }
}
@keyframes load {
 0% {
   stroke-dashoffset: 285;
   animation-timing-function: cubic-bezier(0.550, 0.055, 0.675, 0.190);
 }
 50% {
	 stroke-dashoffset: 0;
	 animation-timing-function: cubic-bezier(0.215, 0.610, 0.355, 1.000);
 }
 100% {
   stroke-dashoffset: -285;
 }
}

@-webkit-keyframes check {
 0% {
   stroke-dashoffset: -110;
}
 100% {
   stroke-dashoffset: 0;
 }
}
@keyframes check {
 0% {
   stroke-dashoffset: -110;
}
 100% {
   stroke-dashoffset: 0;
 }
}

@-webkit-keyframes spin {
  0% {
	-webkit-transform: rotate(0deg);
  }
  100% {
	-webkit-transform: rotate(360deg);
  }
}
@keyframes spin {
  0% {
	transform: rotate(0deg);
  }
  100% {
	transform: rotate(360deg);
  }
}

@-webkit-keyframes spin-fill {
  0% {
	-webkit-transform: rotate(0deg);
  }
  100% {
	-webkit-transform: rotate(720deg);
  }
}
@keyframes spin-fill {
  0% {
	transform: rotate(0deg);
  }
  100% {
	transform: rotate(720deg);
  }
}

@-webkit-keyframes fill {
  0% {
	stroke-dashoffset: 285;
  }
  100% {
	stroke-dashoffset: 0;
  }
}
@keyframes fill {
  0% {
	stroke-dashoffset: 285;
  }
  100% {
	stroke-dashoffset: 0;
  }
}

.change-color 
{
    transition: stroke .5s;
}
</style>
