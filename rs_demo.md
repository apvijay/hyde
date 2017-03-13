---
layout: page
title: Rolling Shutter Demo
---

<!-- -------------------------------------------------------------------------- -->

<!-- Click <i>Load Image</i> and then <i>Warping</i>. Play around with different parameters.  -->
The <i>Warping</i> button will load the parameters and show the distorted output as captured by a rolling shutter camera. If <i>Live update</i> is checked, the slider parameters are updated live. The <i>Reload Image</i> button will show the original undistorted image.

<canvas id="canvas256" width="256" height="256"></canvas>
<canvas id="canvas_exp" width="384" height="128"></canvas>
<canvas id="canvas_mot" width="384" height="128"></canvas>
<div id="buttons">
  <input id="reloadbtn" value="Reload Image" type="button">
  <input id="warpingbtn" value="Warping" type="button">
<!--   <input id="drawbtn" value="Draw" type="button"> -->
</div>
<div id="time_params" >
  <br >
  <input id="warp_cen_check" type="checkbox" value="check"> Warp w.r.t. middle row <br >
  <input id="live_check" type="checkbox" value="check" checked="true"> Live update
  <br ><br >
  <b>Camera Parameters</b>
  <br >
  <input id="flen_box" type="range" min="1" max="1000" step="1" value="30">
  <output id="flen_box_disp">30</output> Focal Length
  <br >
  <input id="t_e_box" type="range" min="1" max="500" step="1" value="1">
  <output id="t_e_box_disp">1</output> Row exposure time
  <br >
  <input id="T_r_box" type="range" min="0" max="100" step="1" value="30">
  <output id="T_r_box_disp">30</output> Top to bottom row delay time
  <br >
  <input id="pose_per_t_e_box" type="range" min="1" max="20" step="1" value="5">
  <output id="pose_per_t_e_box_disp">5</output> Poses per row exposure
  <br >
  <input id="dist_box" type="range" min="1" max="100" step="1" value="1">
  <output id="dist_box_disp">1</output> Scene distance
  </div>
  <div id="mot_params">
  <b>Motion Parameters</b><br >
  <input id="T_e_box" type="range" min="1" max="500" step="1" value="100">
  <output id="T_e_box_disp">100</output> Motion simulation end time
  <br ><br >
  Translations are in
  <input id="trans_unit_mm_box" type="radio" name="trans_unit_check" value="mm">mm, 
  <input id="trans_unit_pix_box" type="radio" name="trans_unit_check" value="pixel" checked="true">pixel
  <br >
  The <i>pixel</i> option translates directly on the image plane and, 
  <br > the focal length and scene distance do not affect them.
  <br ><br >
  <b>Polynomial Camera Motion</b> <input id="zeromotbtn" value="Zero Motion" type="button"><br >
  <!-- &nbsp; &nbsp; &nbsp; &nbsp; &emsp; p0 &nbsp; &nbsp; &nbsp; p1 &nbsp; &nbsp; &nbsp; p2 &nbsp; &nbsp; &nbsp; p3 -->
  <!-- <br > -->
  <p style="color:rgb(180,80,80);display:inline;">oo</p>&emsp;tx
  <input id="tx_p0_box" type="range" min="-100" max="100" size = "4" value="0"> <!--<output id="tx_p0_box_disp">0</output> --> +
  <input id="tx_p1_box" type="text" size = "4" value="-100"> t +
  <input id="tx_p2_box" type="text" size = "4" value="-20"> t^2 +
  <input id="tx_p3_box" type="text" size = "4" value="140"> t^3
  <br >
  <p style="color:rgb(80,180,80);display:inline;">oo</p>&emsp;ty
    <input id="ty_p0_box" type="range" min="-100" max="100" size = "4" value="0"><!--<output id="ty_p0_box_disp">0</output>--> + 
  <input id="ty_p1_box" type="text" size = "4" value="0"> t +
  <input id="ty_p2_box" type="text" size = "4" value="0"> t^2 +
  <input id="ty_p3_box" type="text" size = "4" value="0"> t^3
  <br >
  <p style="color:rgb(80,80,180);display:inline;">oo</p>&emsp;tz
    <input id="tz_p0_box" type="range" min="-10" max="10" size = "4" value="0"><!--<output id="tz_p0_box_disp">0</output> --> + 
  <input id="tz_p1_box" type="text" size = "4" value="0"> t +
  <input id="tz_p2_box" type="text" size = "4" value="0"> t^2 +
  <input id="tz_p3_box" type="text" size = "4" value="0"> t^3
  <br >
  <p style="color:rgb(180,80,80);display:inline;">---</p>&emsp;rx
    <input id="rx_p0_box" type="range" min="-20" max="20" size = "4" value="0"><!-- <output id="rx_p0_box_disp">0</output> --> + 
  <input id="rx_p1_box" type="text" size = "4" value="0"> t +
  <input id="rx_p2_box" type="text" size = "4" value="0"> t^2 +
  <input id="rx_p3_box" type="text" size = "4" value="0"> t^3
  <br >
  <p style="color:rgb(80,180,80);display:inline;">---</p>&emsp;ry
    <input id="ry_p0_box" type="range" min="-20" max="20" size = "4" value="0"><!-- <output id="ry_p0_box_disp">0</output> --> + 
  <input id="ry_p1_box" type="text" size = "4" value="0"> t +
  <input id="ry_p2_box" type="text" size = "4" value="0"> t^2 +
  <input id="ry_p3_box" type="text" size = "4" value="0"> t^3
  <br >
  <p style="color:rgb(80,80,180);display:inline;">---</p>&emsp;rz
    <input id="rz_p0_box" type="range" min="-20" max="20" size = "4" value="0"><!-- <output id="rz_p0_box_disp">0</output> --> + 
  <input id="rz_p1_box" type="text" size = "4" value="0"> t +
  <input id="rz_p2_box" type="text" size = "4" value="0"> t^2 +
  <input id="rz_p3_box" type="text" size = "4" value="0"> t^3
</div>

<script type="text/javascript">



var img = new Image();
var canvas = document.getElementById('canvas256');
var ctx = canvas.getContext('2d');

img.src = '../img/orig256.png';
// ctx.drawImage(img, 0, 0);
// var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
// var img_pix = imageData.data;
// var img_pix_orig = img_pix.slice();

// for (var i = 0; i < 12; i+=1) {
//   console.log(img_pix[i]);
// }
// ctx.putImageData(imageData, 0,0);

  document.getElementById('canvas256').addEventListener("load", reload);

  ctx.drawImage(img, 0,0);
  imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  img_pix = imageData.data;
  img_pix_orig = img_pix.slice();
  ctx.putImageData(imageData, 0,0);

var reload = function() {
  ctx.drawImage(img, 0,0);
  imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  img_pix = imageData.data;
  img_pix_orig = img_pix.slice();
  ctx.putImageData(imageData, 0,0);
}

var zeromot = function() {
  document.getElementById("tx_p0_box").value = 0;
  document.getElementById('ty_p0_box').value = 0;
  document.getElementById('tz_p0_box').value = 0;
  document.getElementById("rx_p0_box").value = 0;
  document.getElementById('ry_p0_box').value = 0;
  document.getElementById('rz_p0_box').value = 0;

  document.getElementById("tx_p1_box").value = 0;
  document.getElementById('ty_p1_box').value = 0;
  document.getElementById('tz_p1_box').value = 0;
  document.getElementById("rx_p1_box").value = 0;
  document.getElementById('ry_p1_box').value = 0;
  document.getElementById('rz_p1_box').value = 0;

  document.getElementById("tx_p2_box").value = 0;
  document.getElementById('ty_p2_box').value = 0;
  document.getElementById('tz_p2_box').value = 0;
  document.getElementById("rx_p2_box").value = 0;
  document.getElementById('ry_p2_box').value = 0;
  document.getElementById('rz_p2_box').value = 0;

  document.getElementById("tx_p3_box").value = 0;
  document.getElementById('ty_p3_box').value = 0;
  document.getElementById('tz_p3_box').value = 0;
  document.getElementById("rx_p3_box").value = 0;
  document.getElementById('ry_p3_box').value = 0;
  document.getElementById('rz_p3_box').value = 0;

  // document.getElementById('tx_p0_box_disp').innerHTML = 0;
  // document.getElementById('ty_p0_box_disp').innerHTML = 0;
  // document.getElementById('tz_p0_box_disp').innerHTML = 0;
  // document.getElementById('rx_p0_box_disp').innerHTML = 0;
  // document.getElementById('ry_p0_box_disp').innerHTML = 0;
  // document.getElementById('rz_p0_box_disp').innerHTML = 0;

}
function getPixelIndex(x, y) {
  return y*imageData.width + x;
}


var apply_homo = function(x,y,H) {
  xd = Number(Math.round((H[0]*x + H[3]*y + H[6]) / (H[2]*x + H[5]*y + H[8])));
  yd = Number(Math.round((H[1]*x + H[4]*y + H[7]) / (H[2]*x + H[5]*y + H[8])));
  return [xd, yd] //yd*imageData.width + xd
}

var get_homo = function(flen,dist,tx,ty,tz,rx,ry,rz) {
  cx = Math.cos(rx);  cy = Math.cos(ry);  cz = Math.cos(rz);
  sx = Math.sin(rx);  sy = Math.sin(ry);  sz = Math.sin(rz);
  H = [0,0,0,0,0,0,0,0,0];
  H[0] = cy * cz;
  H[3] = -cy * sz;
  if(document.getElementById('trans_unit_pix_box').checked == true) {
    H[6] = -sy * flen + tx;
    H[7] = (-sx * cy ) * flen + ty;
    H[8] = cx * cy + tz;
  }else if(document.getElementById('trans_unit_mm_box').checked == true) {
    H[6] = (-sy + tx/dist) * flen;
    H[7] = (-sx * cy + ty/dist) * flen;
    H[8] = cx * cy + tz/dist;
  }
  
  H[1] = cx * sz - sx * sy * cz;
  H[4] = cx * cz + sx * sy * sz;
  
  H[2] = (sx * sz + cx * sy * cz) / flen;
  H[5] = (sx * cz - cx * sy * sz) / flen;
  
  return H;

}
document.getElementById("flen_box").oninput = function() {
  val = document.getElementById("flen_box").value;
  document.getElementById('flen_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("T_r_box").oninput = function() {
  val = document.getElementById("T_r_box").value;
  document.getElementById('T_r_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("t_e_box").oninput = function() {
  val = document.getElementById("t_e_box").value;
  document.getElementById('t_e_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("pose_per_t_e_box").oninput = function() {
  val = document.getElementById("pose_per_t_e_box").value;
  document.getElementById('pose_per_t_e_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("dist_box").oninput = function() {
  val = document.getElementById("dist_box").value;
  document.getElementById('dist_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("T_e_box").oninput = function() {
  val = document.getElementById("T_e_box").value;
  document.getElementById('T_e_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};

document.getElementById("tx_p0_box").oninput = function() {
  // val = document.getElementById("tx_p0_box").value;
  // document.getElementById('tx_p0_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("ty_p0_box").oninput = function() {
  // val = document.getElementById("ty_p0_box").value;
  // document.getElementById('ty_p0_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("tz_p0_box").oninput = function() {
  // val = document.getElementById("tz_p0_box").value;
  // document.getElementById('tz_p0_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("rx_p0_box").oninput = function() {
  // val = document.getElementById("rx_p0_box").value;
  // document.getElementById('rx_p0_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("ry_p0_box").oninput = function() {
  // val = document.getElementById("ry_p0_box").value;
  // document.getElementById('ry_p0_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("rz_p0_box").oninput = function() {
  // val = document.getElementById("rz_p0_box").value;
  // document.getElementById('rz_p0_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};

var rsmb_warp = function() {
  // exp_draw();

  //if(document.getElementById('trans_unit_pix_box').checked == true)

  var flen = Number(document.getElementById('flen_box').value);
  var t_e = Number(document.getElementById('t_e_box').value);
  var T_r = Number(document.getElementById('T_r_box').value);
  var pose_per_te = Number(document.getElementById('pose_per_t_e_box').value);
  var T_e = Number(document.getElementById('T_e_box').value);

  var dist = Number(document.getElementById('dist_box').value);

  var tx_p0 = Number(document.getElementById('tx_p0_box').value);
  var tx_p1 = Number(document.getElementById('tx_p1_box').value);
  var tx_p2 = Number(document.getElementById('tx_p2_box').value);
  var tx_p3 = Number(document.getElementById('tx_p3_box').value);
  var ty_p0 = Number(document.getElementById('ty_p0_box').value);
  var ty_p1 = Number(document.getElementById('ty_p1_box').value);
  var ty_p2 = Number(document.getElementById('ty_p2_box').value);
  var ty_p3 = Number(document.getElementById('ty_p3_box').value);
  var tz_p0 = Number(document.getElementById('tz_p0_box').value);
  var tz_p1 = Number(document.getElementById('tz_p1_box').value);
  var tz_p2 = Number(document.getElementById('tz_p2_box').value);
  var tz_p3 = Number(document.getElementById('tz_p3_box').value);
  var rx_p0 = Number(document.getElementById('rx_p0_box').value);
  var rx_p1 = Number(document.getElementById('rx_p1_box').value);
  var rx_p2 = Number(document.getElementById('rx_p2_box').value);
  var rx_p3 = Number(document.getElementById('rx_p3_box').value);
  var ry_p0 = Number(document.getElementById('ry_p0_box').value);
  var ry_p1 = Number(document.getElementById('ry_p1_box').value);
  var ry_p2 = Number(document.getElementById('ry_p2_box').value);
  var ry_p3 = Number(document.getElementById('ry_p3_box').value);
  var rz_p0 = Number(document.getElementById('rz_p0_box').value);
  var rz_p1 = Number(document.getElementById('rz_p1_box').value);
  var rz_p2 = Number(document.getElementById('rz_p2_box').value);
  var rz_p3 = Number(document.getElementById('rz_p3_box').value);

//  document.getElementById('t_e_box_disp').innerHTML = t_e;

//  var t_e = 10; // single row exposure in ms
//  var T_r = 30; // total line delay in ms
  var t_r = T_r / (imageData.height-1); // single line delay
//  var pose_per_te = 10;
  // var T_e = 100; //t_e + T_r; // total image exposure

        yidx_cen =   (imageData.height/2 * t_r) / T_e;
  if (document.getElementById('warp_cen_check').checked == true) {
    tx_cen = tx_p0 + (tx_p1 * yidx_cen) + (tx_p2 * Math.pow(yidx_cen,2)) + (tx_p3 * Math.pow(yidx_cen,3));
    ty_cen = ty_p0 + ty_p1 * yidx_cen + ty_p2 * Math.pow(yidx_cen,2) + ty_p3 * Math.pow(yidx_cen,3);
    tz_cen = tz_p0 + tz_p1 * yidx_cen + tz_p2 * Math.pow(yidx_cen,2) + tz_p3 * Math.pow(yidx_cen,3);
    rx_cen = (rx_p0 + rx_p1 * yidx_cen + rx_p2 * Math.pow(yidx_cen,2) + rx_p3 * Math.pow(yidx_cen,3))*Math.PI/180;
    ry_cen = (ry_p0 + ry_p1 * yidx_cen + ry_p2 * Math.pow(yidx_cen,2) + ry_p3 * Math.pow(yidx_cen,3))*Math.PI/180;
    rz_cen = (rz_p0 + rz_p1 * yidx_cen + rz_p2 * Math.pow(yidx_cen,2) + rz_p3 * Math.pow(yidx_cen,3))*Math.PI/180;
  }
  
  for (y = 0; y < imageData.height; y += 1) {
    for (n = 0; n < pose_per_te; n += 1) {

      yidx =   (y * t_r + n * t_e/pose_per_te) / T_e; // (1/pose_per_te) * // (y + n/pose_per_te);

      tx = tx_p0 + (tx_p1 * yidx) + (tx_p2 * Math.pow(yidx,2)) + (tx_p3 * Math.pow(yidx,3));
      ty = ty_p0 + ty_p1 * yidx + ty_p2 * Math.pow(yidx,2) + ty_p3 * Math.pow(yidx,3);
      tz = tz_p0 + tz_p1 * yidx + tz_p2 * Math.pow(yidx,2) + tz_p3 * Math.pow(yidx,3);
      rx = (rx_p0 + rx_p1 * yidx + rx_p2 * Math.pow(yidx,2) + rx_p3 * Math.pow(yidx,3))*Math.PI/180;
      ry = (ry_p0 + ry_p1 * yidx + ry_p2 * Math.pow(yidx,2) + ry_p3 * Math.pow(yidx,3))*Math.PI/180;
      rz = (rz_p0 + rz_p1 * yidx + rz_p2 * Math.pow(yidx,2) + rz_p3 * Math.pow(yidx,3))*Math.PI/180;

      if (document.getElementById('warp_cen_check').checked == true) {
        tx = tx - tx_cen;
        ty = ty - ty_cen;
        tz = tz - tz_cen;
        rx = rx - rx_cen;
        ry = ry - ry_cen;
        rz = rz - rz_cen;
      }
      for (x = 0; x < imageData.width; x += 1) {
        H = get_homo(flen,dist,-tx,-ty,-tz,-rx,-ry,-rz);
        // if(x==0 && y==0 & n==0) console.log(H,tx,flen,dist);
        tmp = apply_homo(x-imageData.width/2,y-imageData.height/2, H);
        xd = tmp[0]+imageData.width/2;
        yd = tmp[1]+imageData.height/2;
        
        i = getPixelIndex(x,y);
        j = getPixelIndex(xd,yd);

        if (n == 0) { 
          if (xd >=0 && yd >= 0 && xd < imageData.width && yd < imageData.height) {
            img_pix[4*i] = img_pix_orig[4*j] / pose_per_te; 
            img_pix[4*i+1] = img_pix_orig[4*j+1] / pose_per_te; 
            img_pix[4*i+2] = img_pix_orig[4*j+2] / pose_per_te; 
          }
          else {
            img_pix[4*i] = 120 / pose_per_te;
            img_pix[4*i+1] = 0 / pose_per_te;
            img_pix[4*i+2] = 0 / pose_per_te;
          }
        } 
        else {
          if (xd >=0 && yd >= 0 && xd < imageData.width && yd < imageData.height) {
            img_pix[4*i] += img_pix_orig[4*j] / pose_per_te; 
            img_pix[4*i+1] += img_pix_orig[4*j+1] / pose_per_te;
            img_pix[4*i+2] += img_pix_orig[4*j+2] / pose_per_te; 
          }
          else {
            img_pix[4*i] += 120 / pose_per_te;
            img_pix[4*i+1] += 0 / pose_per_te;
            img_pix[4*i+2] += 0 / pose_per_te;
          }
        } // end if n == 0
       
      } // end for x
    } // end for n
  } // end for y
  
  ctx.putImageData(imageData, 0,0);

  canvas_mot_width = 384;
  FAC = canvas_mot_width / T_e;
  canvas_exp = document.getElementById('canvas_exp');
  if (canvas_exp.getContext) {
    ctx_exp = canvas_exp.getContext('2d');
    ctx_exp.clearRect(0, 0, canvas_mot_width, 128);

    ctx_exp.fillStyle = 'rgb(240,240,210)';
    ctx_exp.fillRect(0, 0, canvas_mot_width, 128);
    // ctx_exp.clearRect(45, 45, 60, 60);
    // ctx_exp.strokeRect(50, 50, 50, 50);
    console.log(0,0, T_r * FAC, 128, (T_r+t_e) * FAC, 128, t_e * FAC, 0, 0, 0);
    ctx_exp.translate(0,0);
    ctx_exp.strokeStyle = "rgb(180, 170, 120)";
    ctx_exp.fillStyle = "rgb(180, 170, 120)";
    ctx_exp.beginPath();
    ctx_exp.lineTo(T_r * FAC, 128);
    ctx_exp.lineTo((T_r+t_e) * FAC, 128);
    ctx_exp.lineTo(t_e * FAC, 0);
    ctx_exp.lineTo(0, 0);
    ctx_exp.closePath();
    ctx_exp.fillStyle = "rgb(180, 170, 120)";
    ctx_exp.fill();
    // ctx_exp.lineWidth = 1.0;
    //ctx_exp.lineCap = "round";
    //ctx_exp.lineJoin = "round";
    // ctx_exp.stroke();

    // ctx_exp.beginPath();
    // ctx_exp.strokeStyle = 'rgb(150,150,150)';
    // ctx_exp.moveTo(1,0);    
    // ctx_exp.lineTo(1,128);
    // ctx_exp.lineWidth = 1.0;

    ctx_exp.strokeStyle = 'rgb(80,80,0)';
    ctx_exp.fillStyle = 'rgb(150,150,150)';
    ctx_exp.font = 'italic 10pt PT Sans';
    ctx_exp.strokeText("top row",15,15);
    ctx_exp.strokeText("bot row",15,125);
    ctx_exp.font = '12pt PT Sans';
    ctx_exp.strokeText("Exposure",canvas_mot_width/2-30,15);
    // ctx_exp.strokeText(100,100,61);
    // ctx_exp.strokeText(50,50,61);
    // ctx_exp.strokeText(20,20,61);

    ctx_exp.stroke();
  }
  
  canvas_mot = document.getElementById('canvas_mot');
  if (canvas_mot.getContext) {
    ctx_mot = canvas_mot.getContext('2d');
    ctx_mot.clearRect(0, 0, canvas_mot_width, 128);

    // ctx_mot.translate(0,0);

    ctx_mot.fillStyle = 'rgb(240,240,230)';
    ctx_mot.fillRect(0, 0, canvas_mot_width, 128);
    ctx_mot.fillStyle = 'rgb(120,120,0)';
    

    if (t_e > T_r) {
      ctx_mot.fillStyle = 'rgb(220,220,210)';
      ctx_mot.fillRect(0 * FAC, 0, (T_r-0) * FAC, 128-0);
      ctx_mot.fillStyle = 'rgb(210,210,190)';
      ctx_mot.fillRect(T_r * FAC, 0, (t_e-T_r) * FAC, 128-0);
      ctx_mot.fillStyle = 'rgb(220,220,210)';
      ctx_mot.fillRect(t_e * FAC, 0, (t_e+T_r-t_e) * FAC, 128-0);
    }
    
    ctx_mot.beginPath();
    ctx_mot.strokeStyle = 'rgb(150,150,150)';
    ctx_mot.moveTo(0,64);    
    ctx_mot.lineTo(canvas_mot_width,64);
    ctx_mot.lineWidth = 1.0;
    ctx_mot.stroke();

    
    for (t = 0; t < T_e; t+=1) {
      tx_plot = tx_p0 + (tx_p1 * t/T_e) + (tx_p2 * Math.pow(t/T_e,2)) + (tx_p3 * Math.pow(t/T_e,3));
      if (t == 0) {
        tx_max_plot = tx_plot;
        tx_min_plot = tx_plot;
      }
      else {
        if (tx_plot > tx_max_plot) tx_max_plot = tx_plot;
        if (tx_plot < tx_min_plot) tx_min_plot = tx_plot;
      }
      tx_plot = 64 - tx_plot; // 64->0, -64->128
      ctx_mot.strokeStyle = 'rgb(180,80,80)';
      ctx_mot.beginPath();
      // ctx_mot.lineTo(t * FAC, tx_plot);
      ctx_mot.arc(t * FAC, tx_plot, 1.2, 0, Math.PI * 2, true);   
      ctx_mot.closePath();
      ctx_mot.stroke();

      ty_plot = ty_p0 + (ty_p1 * t/T_e) + (ty_p2 * Math.pow(t/T_e,2)) + (ty_p3 * Math.pow(t/T_e,3));
      ty_plot = 64 - ty_plot; // 64->0, -64->128
      ctx_mot.strokeStyle = 'rgb(80,180,80)';
      ctx_mot.beginPath();
      // ctx_mot.lineTo(t * FAC, tx_plot);
      ctx_mot.arc(t * FAC, ty_plot, 1.2, 0, Math.PI * 2, true);   
      ctx_mot.closePath();
      ctx_mot.stroke();

      tz_plot = tz_p0 + (tz_p1 * t/T_e) + (tz_p2 * Math.pow(t/T_e,2)) + (tz_p3 * Math.pow(t/T_e,3));
      tz_plot = 64 - tz_plot; // 64->0, -64->128
      ctx_mot.strokeStyle = 'rgb(80,80,180)';
      ctx_mot.beginPath();
      // ctx_mot.lineTo(t * FAC, tx_plot);
      ctx_mot.arc(t * FAC, tz_plot, 1.2, 0, Math.PI * 2, true);   
      ctx_mot.closePath();
      ctx_mot.stroke();
    }

    ctx_mot.beginPath();
    ctx_mot.strokeStyle = 'rgb(180,80,80)';
    ctx_mot.moveTo(0,64);
    for (t = 0; t < T_e; t+=1) {
      rx_plot = rx_p0 + (rx_p1 * t/T_e) + (rx_p2 * Math.pow(t/T_e,2)) + (rx_p3 * Math.pow(t/T_e,3));
      rx_plot = 64 - rx_plot*64/20; // 20->0, 0->64, -20->128 //  64->0, -64->128
      // ctx_mot.lineTo(t * FAC, tx_plot);
      ctx_mot.arc(t * FAC, rx_plot, 0.4, 0, Math.PI * 2, true);   
      // ctx_mot.lineWidth = 1.0;
      // ctx_mot.closePath();  
    }
    ctx_mot.stroke();

    ctx_mot.beginPath();
    ctx_mot.strokeStyle = 'rgb(80,180,80)';
    ctx_mot.moveTo(0,64);
    for (t = 0; t < T_e; t+=1) {
      ry_plot = ry_p0 + (ry_p1 * t/T_e) + (ry_p2 * Math.pow(t/T_e,2)) + (ry_p3 * Math.pow(t/T_e,3));
      ry_plot = 64 - ry_plot*64/20; // 64->0, -64->128
      // ctx_mot.lineTo(t * FAC, tx_plot);
      ctx_mot.arc(t * FAC, ry_plot, 0.4, 0, Math.PI * 2, true);   
      ctx_mot.lineWidth = 1.0;
      // ctx_mot.closePath();      
    }
    ctx_mot.stroke();

    ctx_mot.beginPath();
    ctx_mot.strokeStyle = 'rgb(80,80,180)';
    ctx_mot.moveTo(0,64);
    for (t = 0; t < T_e; t+=1) {
      rz_plot = rz_p0 + (rz_p1 * t/T_e) + (rz_p2 * Math.pow(t/T_e,2)) + (rz_p3 * Math.pow(t/T_e,3));
      rz_plot = 64 - rz_plot*64/20; // 64->0, -64->128     
      ctx_mot.arc(t * FAC, rz_plot, 0.4, 0, Math.PI * 2, true);
      // cts_mot.lineTo(t*FAC,rz_plot);
      ctx_mot.lineWidth = 1.0;
      // ctx_mot.closePath();
    }
    ctx_mot.stroke();

    ctx_mot.moveTo(0,64);
    ctx_mot.strokeStyle = 'rgb(150,150,150)';
    ctx_mot.font = 'italic 12pt PT Sans';
    ctx_mot.strokeText("0",5,61);
    ctx_mot.strokeText(T_e,canvas_mot_width-30,61);
    ctx_mot.strokeText("time",canvas_mot_width-30,75);
    // ctx_mot.strokeText(100,100,61);
    // ctx_mot.strokeText(50,50,61);
    // ctx_mot.strokeText(20,20,61);
    ctx_mot.stroke();

    ctx_mot.moveTo(0,64);
    ctx_mot.font = 'italic 10pt PT Sans';
    if(document.getElementById('trans_unit_pix_box').checked == true) {
      ctx_mot.strokeText("64pix",5,15);
      ctx_mot.strokeText("-64pix",5,120);
    }
    else {
      ctx_mot.strokeText("64mm",5,15);
      ctx_mot.strokeText("-64mm",5,120);
    }
    
    ctx_mot.strokeText("20\u00B0",canvas_mot_width-30,15);
    ctx_mot.strokeText("-20\u00B0",canvas_mot_width-30,120);
    ctx_mot.font = '12pt PT Sans';
    ctx_mot.strokeText("Camera Motion",canvas_mot_width/2-50,15);
    ctx_mot.stroke();
    
  }
}


var warpingbtn = document.getElementById('warpingbtn');
warpingbtn.addEventListener('click', rsmb_warp);
var reloadbtn = document.getElementById('reloadbtn');
reloadbtn.addEventListener('click', reload);
// var drawbtn = document.getElementById('drawbtn');
// drawbtn.addEventListener('click', exp_draw);
var zeromotbtn = document.getElementById('zeromotbtn');
zeromotbtn.addEventListener('click', zeromot);
</script>


<!---------------------------------------------------------------------------->