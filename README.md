<html>
<!-- README -->
<head>
    <style>
        .button {
          border: none;
          color: white;
          padding: 15px 32px;
          text-align: center;
          text-decoration: none;
          display: inline-block;
          font-size: 16px;
          margin: 4px 2px;
          cursor: pointer;
        }
        
        .button1 {background-color: #04AA6D;} /* Green */
    
    </style>
</head>
<h1>Send Gyro Data!</h1>
<input id="IP">
<div>Orientation:</div>
<p id="ori"></p>

<script>
a = 0
b = 0
c = 0
no_timer = true
ip = ""

window.addEventListener("deviceorientation", handleOrientation);
try {
    if (typeof DeviceMotionEvent.requestPermission === 'function') {
    // Handle iOS 13+ devices.
    DeviceMotionEvent.requestPermission()
        .then((state) => {
        if (state === 'granted') {
            window.addEventListener('devicemotion', handleMotion);
        } else {
            console.error('Request to access the orientation was rejected');
        }
        })
        .catch(console.error);
    } else {
    // Handle regular non iOS 13+ devices.
    window.addEventListener('devicemotion', handleMotion2);
    }
} catch (error) {
    console.log(error)
}
function handleOrientation(event) {
    
    if (event.alpha === null) {
      return
    }
    const alpha = event.alpha;
    const beta = event.beta;
    const gamma = event.gamma;
    el.innerHTML = "a,b,c: " +alpha +", "+beta+", "+gamma
    a = alpha
    b = beta
    c = gamma
  }
  function handleMotion(event) {
      if (event.alpha === null) {
      return
    }
    console.log(event.alpha, event.beta, event.gamma)
    const alpha = event.alpha;
    const beta = event.beta;
    const gamma = event.gamma;
    el.innerHTML = "a,b,c: " +alpha +", "+beta+", "+gamma
    a = alpha
    b = beta
    c = gamma
  }
  function send(data) {
    const xhr = new XMLHttpRequest();
    xhr.open("POST", "https://"+ip+":8080/trans/target");
    xhr.setRequestHeader("Content-Type", "application/json; charset=UTF-8")
    const body = JSON.stringify(data);
    xhr.send(body);
}
function sendGyro(){
    send({
        alpha: a,
        beta: b,
        gamma: c})
}
  
  function onClick() {
    ip = document.getElementById("IP").value
    document.getElementById("o").innerHTML = ip
    if (no_timer){
        no_timer = false
        setInterval(sendGyro, 5000)
    }
  }
</script>
<p id="o">fh</p>
<button class="button button1" onclick="onClick()">Load IP</button>

</html>
