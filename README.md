# 文字图片粒子化
> 主要应用canvasctx.getImageData()获取像素值，将符合的像素再画到画布上
## 文字粒子化
> 使用ctx.fillText(text,w.h)将文字画至画布上。

    canvas.ctx.fillText(text.obj.value,canvas.w/2,canvas.h/2)
    text.data = canvas.ctx.getImageData(0,0,canvas.w,canvas.h);

> canvas.wigth = w;canvas.height = h;  canvas.ctx.getImageData是一个Uint8ClampedArray类型的一维数组，包含了整个图片区域里每个像素点的RGBA的整型数据，所以要取某一点n的位置：pos = i+j*w；    
>nr = pos*4;ng = pos*4+1;
nb = pos*4+2;na = pos*4+3
>拿到每一点的每一个像素

    for (var i = 0; i < canvas.w; i++) {
        for (var j = 0; j < canvas.h; j++) {
          pos = (j*canvas.w + i)*4;
          if(data.data[pos+3]>128 && pos%9 == 0){
            var particle = {
              x:(i-3)+Math.random()*6,
              y:(j-3)+Math.random()*6,
            }
            particles.push(particle)
          }
        }
      }
> 符合标准的一个点的位置存入数组中

    function draw(){
      canvas.ctx.clearRect(0,0,canvas.w,canvas.h);
      var len = particles.length;
      var curr_particle = null;
      for (var i = 0; i < len; i++) {
        curr_particle = particles[i];
        canvas.ctx.fillRect(curr_particle.x,curr_particle.y,1,1)
      }
    }
> 再将particles数组中的每一个点画出来。

## 图形粒子化

> 图形与文字思路相似，但是在绘制过程中，将图片绘制在画布上后，ctx.getImageData会报错，因为html的域名与图片域名不同导致canvas跨域问题。  
> 解决：在本地搭建服务器，使用相同域名，避免跨域

> 其次，图片在粒子化时，需要保存某一点的像素值

    var particle = {
        x:image.x+i*s_width+(Math.random()-0.5)*10,
        y:image.y+j*s_height+(Math.random()-0.5)*10,
        fillStyle:'rgba('+data[pos]+','+data[pos+1]+','+data[pos+2]+','+data[pos+3]+')'
      }
> 在绘制时也使用            canvas.ctx.fillStyle = curr_particle.fillStyle;改变某点颜色值。
