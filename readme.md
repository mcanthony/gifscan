GIFScan
==========
Live slitscanning of animated gifs

To use:
1. Drag this link to your bookmarks bar: [GIFscan](var bitsToNum=function(e\){return e.reduce(function(e,t\){return e*2+t},0\)};var byteToBitArr=function(e\){var t=[];for(var n=7;n>=0;n--\){t.push(!!(e&1<<n\)\)}return t};var Stream=function(e\){this.data=e;this.len=this.data.length;this.pos=0;this.readByte=function(\){if(this.pos>=this.data.length\){throw new Error("Attempted to read past end of stream."\)}return e.charCodeAt(this.pos++\)&255};this.readBytes=function(e\){var t=[];for(var n=0;n<e;n++\){t.push(this.readByte(\)\)}return t};this.read=function(e\){var t="";for(var n=0;n<e;n++\){t+=String.fromCharCode(this.readByte(\)\)}return t};this.readUnsigned=function(\){var e=this.readBytes(2\);return(e[1]<<8\)+e[0]}};var lzwDecode=function(e,t\){var n=0;var r=function(e\){var r=0;for(var i=0;i<e;i++\){if(t.charCodeAt(n>>3\)&1<<(n&7\)\){r|=1<<i}n++}return r};var i=[];var s=1<<e;var o=s+1;var u=e+1;var a=[];var f=function(\){a=[];u=e+1;for(var t=0;t<s;t++\){a[t]=[t]}a[s]=[];a[o]=null};var l;var c;while(true\){c=l;l=r(u\);if(l===s\){f(\);continue}if(l===o\)break;if(l<a.length\){if(c!==s\){a.push(a[c].concat(a[l][0]\)\)}}else{if(l!==a.length\)throw new Error("Invalid LZW code."\);a.push(a[c].concat(a[c][0]\)\)}i.push.apply(i,a[l]\);if(a.length===1<<u&&u<12\){u++}}return i};var parseGIF=function(e,t\){t||(t={}\);var n=function(t\){var n=[];for(var r=0;r<t;r++\){n.push(e.readBytes(3\)\)}return n};var r=function(\){var t,n;n="";do{t=e.readByte(\);n+=e.read(t\)}while(t!==0\);return n};var i=function(\){var r={};r.sig=e.read(3\);r.ver=e.read(3\);if(r.sig!=="GIF"\)throw new Error("Not a GIF file."\);r.width=e.readUnsigned(\);r.height=e.readUnsigned(\);var i=byteToBitArr(e.readByte(\)\);r.gctFlag=i.shift(\);r.colorRes=bitsToNum(i.splice(0,3\)\);r.sorted=i.shift(\);r.gctSize=bitsToNum(i.splice(0,3\)\);r.bgColor=e.readByte(\);r.pixelAspectRatio=e.readByte(\);if(r.gctFlag\){r.gct=n(1<<r.gctSize+1\)}t.hdr&&t.hdr(r\)};var s=function(n\){var i=function(n\){var r=e.readByte(\);var i=byteToBitArr(e.readByte(\)\);n.reserved=i.splice(0,3\);n.disposalMethod=bitsToNum(i.splice(0,3\)\);n.userInput=i.shift(\);n.transparencyGiven=i.shift(\);n.delayTime=e.readUnsigned(\);n.transparencyIndex=e.readByte(\);n.terminator=e.readByte(\);t.gce&&t.gce(n\)};var s=function(e\){e.comment=r(\);t.com&&t.com(e\)};var o=function(n\){var i=e.readByte(\);n.ptHeader=e.readBytes(12\);n.ptData=r(\);t.pte&&t.pte(n\)};var u=function(n\){var i=function(n\){var r=e.readByte(\);n.unknown=e.readByte(\);n.iterations=e.readUnsigned(\);n.terminator=e.readByte(\);t.app&&t.app.NETSCAPE&&t.app.NETSCAPE(n\)};var s=function(e\){e.appData=r(\);t.app&&t.app[e.identifier]&&t.app[e.identifier](e\)};var o=e.readByte(\);n.identifier=e.read(8\);n.authCode=e.read(3\);switch(n.identifier\){case"NETSCAPE":i(n\);break;default:s(n\);break}};var a=function(e\){e.data=r(\);t.unknown&&t.unknown(e\)};n.label=e.readByte(\);switch(n.label\){case 249:n.extType="gce";i(n\);break;case 254:n.extType="com";s(n\);break;case 1:n.extType="pte";o(n\);break;case 255:n.extType="app";u(n\);break;default:n.extType="unknown";a(n\);break}};var o=function(i\){var s=function(e,t\){var n=new Array(e.length\);var r=e.length/t;var i=function(r,i\){var s=e.slice(i*t,(i+1\)*t\);n.splice.apply(n,[r*t,t].concat(s\)\)};var s=[0,4,2,1];var o=[8,8,4,2];var u=0;for(var a=0;a<4;a++\){for(var f=s[a];f<r;f+=o[a]\){i(f,u\);u++}}return n};i.leftPos=e.readUnsigned(\);i.topPos=e.readUnsigned(\);i.width=e.readUnsigned(\);i.height=e.readUnsigned(\);var o=byteToBitArr(e.readByte(\)\);i.lctFlag=o.shift(\);i.interlaced=o.shift(\);i.sorted=o.shift(\);i.reserved=o.splice(0,2\);i.lctSize=bitsToNum(o.splice(0,3\)\);if(i.lctFlag\){i.lct=n(1<<i.lctSize+1\)}i.lzwMinCodeSize=e.readByte(\);var u=r(\);i.pixels=lzwDecode(i.lzwMinCodeSize,u\);if(i.interlaced\){i.pixels=s(i.pixels,i.width\)}t.img&&t.img(i\)};var u=function(\){var n={};n.sentinel=e.readByte(\);switch(String.fromCharCode(n.sentinel\)\){case"!":n.type="ext";s(n\);break;case",":n.type="img";o(n\);break;case";":n.type="eof";t.eof&&t.eof(n\);break;default:throw new Error("Unknown block: 0x"+n.sentinel.toString(16\)\)}if(n.type!=="eof"\)setTimeout(u,0\)};var a=function(\){i(\);setTimeout(u,0\)};a(\)};var SuperGif=function(e\){var t={vp_l:0,vp_t:0,vp_w:null,vp_h:null,c_w:null,c_h:null};for(var n in e\){t[n]=e[n]}if(t.vp_w&&t.vp_h\)t.is_vp=true;var r;var i;var s=null;var o=false;var u=null;var a=null;var f=null;var l=0;var c=null;var h=null;var p=null;var d=true;var v=true;var m=false;var g=[];var y=t.gif;if(typeof t.auto_play=="undefined"\)t.auto_play=!y.getAttribute("rel:auto_play"\)||y.getAttribute("rel:auto_play"\)=="1";var b=function(\){u=null;a=null;c=f;f=null;h=null};var w=function(\){try{parseGIF(r,D\)}catch(e\){T("parse"\)}};var E=function(e\){F.innerHTML=e;F.style.visibility="visible"};var S=function(e,t\){B.width=e*H(\);B.height=t*H(\);F.style.minWidth=e*H(\)+"px";I.width=e;I.height=t;I.style.width=e+"px";I.style.height=t+"px";I.getContext("2d"\).setTransform(1,0,0,1,0,0\)};var x=function(e,n,r\){if(r\){var i=25;var s,o,u,a;if(t.is_vp\){if(!m\){u=t.vp_t+t.vp_h-i;i=i;s=t.vp_l;o=s+e/n*t.vp_w;a=B.width}else{u=(t.vp_t+t.vp_h-i\)/H(\);i=i/H(\);s=t.vp_l/H(\);o=s+e/n*(t.vp_w/H(\)\);a=B.width/H(\)}if(false\){if(!m\){var f=t.vp_l,l=t.vp_t;var c=t.vp_w,h=t.vp_h}else{var f=t.vp_l/H(\),l=t.vp_t/H(\);var c=t.vp_w/H(\),h=t.vp_h/H(\)}j.rect(f,l,c,h\);j.stroke(\)}}else{u=B.height-i;o=e/n*B.width;a=B.width}j.fillStyle="rgba(255,255,255,0.4\)";j.fillRect(o,u,a-o,i\);j.fillStyle="rgba(255,0,22,.8\)";j.fillRect(0,u,o,i\)}};var T=function(e\){var n=function(\){j.fillStyle="black";j.fillRect(0,0,t.c_w?t.c_w:i.width,t.c_h?t.c_h:i.height\);j.strokeStyle="red";j.lineWidth=3;j.moveTo(0,0\);j.lineTo(t.c_w?t.c_w:i.width,t.c_h?t.c_h:i.height\);j.moveTo(0,t.c_h?t.c_h:i.height\);j.lineTo(t.c_w?t.c_w:i.width,0\);j.stroke(\)};s=e;i={width:y.width,height:y.height};g=[];n(\)};var N=function(e\){i=e;S(i.width,i.height\)};var C=function(e\){k(\);b(\);u=e.transparencyGiven?e.transparencyIndex:null;a=e.delayTime;f=e.disposalMethod};var k=function(\){if(!h\)return;g.push({data:h.getImageData(0,0,i.width,i.height\),delay:a}\)};var L=function(e\){if(!h\)h=I.getContext("2d"\);var t=g.length;var n=e.lctFlag?e.lct:i.gct;if(t>0\){if(c===3\){h.putImageData(g[l].data,0,0\)}else{l=t-1}if(c===2\){h.clearRect(p.leftPos,p.topPos,p.width,p.height\)}}var r=h.getImageData(e.leftPos,e.topPos,e.width,e.height\);var s=r.data;e.pixels.forEach(function(e,t\){if(e!==u\){s[t*4+0]=n[e][0];s[t*4+1]=n[e][1];s[t*4+2]=n[e][2];s[t*4+3]=255}}\);r.data=s;h.putImageData(r,e.leftPos,e.topPos\);if(!m\){j.scale(H(\),H(\)\);m=true}j.drawImage(I,0,0\);p=e};var A=function(\){var e=-1;var n;var r;var i=false;var o=false;var u=function(t\){e=(e+t+g.length\)%g.length;n=e+1;r=g[e].delay;f(\)};var a=function(\){var t=false;var n=function(\){t=d;if(!t\)return;u(v?1:-1\);var r=g[e].delay*10;if(!r\)r=100;setTimeout(n,r\)};return function(\){if(!t\)setTimeout(n,0\)}}(\);var f=function(\){n=e;I.getContext("2d"\).putImageData(g[e].data,0,0\);j.globalCompositeOperation="copy";j.drawImage(I,0,0\)};var l=function(\){d=true;a(\)};var c=function(\){d=false};return{init:function(\){if(s\)return;if(!(t.c_w&&t.c_h\)\){j.scale(H(\),H(\)\)}if(t.auto_play\){a(\)}else{e=0;f(\)}},current_frame:n,step:a,play:l,pause:c,playing:d,move_relative:u,current_frame:function(\){return e},length:function(\){return g.length},move_to:function(t\){e=t;f(\)}}}(\);var O=function(e\){x(r.pos,r.data.length,e\)};var M=function(\){};var _=function(e,t\){return function(n\){e(n\);O(t\)}};var D={hdr:_(N\),gce:_(C\),com:_(M\),app:{NETSCAPE:_(M\)},img:_(L,true\),eof:function(e\){k(\);O(false\);if(!(t.c_w&&t.c_h\)\){B.width=i.width*H(\);B.height=i.height*H(\)}A.init(\);o=false;if(R\){R(\)}}};var P=function(\){var e=y.parentNode;var n=document.createElement("div"\);B=document.createElement("canvas"\);j=B.getContext("2d"\);F=document.createElement("div"\);I=document.createElement("canvas"\);n.width=B.width=y.width;n.height=B.height=y.height;F.style.minWidth=y.width+"px";n.className="jsgif";F.className="jsgif_toolbar";n.appendChild(B\);n.appendChild(F\);e.insertBefore(n,y\);e.removeChild(y\);if(t.c_w&&t.c_h\)S(t.c_w,t.c_h\)};var H=function(\){var e;if(t.max_width&&i\){e=t.max_width/i.width}else{e=1}return e};var B,j,F,I;var q=false;var R=false;return{play:A.play,pause:A.pause,move_relative:A.move_relative,move_to:A.move_to,get_frames:function(\){return g},get_playing:function(\){return A.playing},get_canvas:function(\){return B},get_canvas_scale:function(\){return H(\)},get_loading:function(\){return o},get_auto_play:function(\){return t.auto_play},get_length:function(\){return A.length(\)},get_current_frame:function(\){return A.current_frame(\)},load:function(e\){if(e\)R=e;o=true;var t=new XMLHttpRequest;t.overrideMimeType("text/plain; charset=x-user-defined"\);t.onloadstart=function(\){if(!q\)P(\)};t.onload=function(e\){r=new Stream(t.responseText\);setTimeout(w,0\)};t.onprogress=function(e\){if(e.lengthComputable\)x(e.loaded,e.total,true\)};t.onerror=function(\){T("xhr"\)};t.open("GET",y.getAttribute("rel:animated_src"\)||y.src,true\);t.send(\)}}};if(!window.getQueryString\){window.getQueryString=function(e,t\){if(t==null\)t="";e=e.replace(/[\[]/,"\\["\).replace(/[\]]/,"\\]"\);var n=new RegExp("[\\?&]"+e+"=([^&#]*\)"\);var r=n.exec(window.location.href\);if(r==null\)return t;else return r[1]}}if(!window.requestAnimationFrame\){window.requestAnimationFrame=function(\){return window.webkitRequestAnimationFrame||window.mozRequestAnimationFrame||window.oRequestAnimationFrame||window.msRequestAnimationFrame||function(e,t\){window.setTimeout(e,1e3/60\)}}(\)}if(!Function.prototype.bind\){Function.prototype.bind=function(e\){if(typeof this!=="function"\){throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable"\)}var t=Array.prototype.slice.call(arguments,1\),n=this,r=function(\){},i=function(\){return n.apply(this instanceof r?this:e||window,t.concat(Array.prototype.slice.call(arguments\)\)\)};r.prototype=this.prototype;i.prototype=new r;return i}}var GifParser=function(e\){function n(e,t\){if(e.data.length!=t.data.length\)return false;for(var n=0;n<e.data.length;++n\){if(e.data[n]!=t.data[n]\)return false}return true}this.frames=[];var t=null;this.load=function(e\){this.canvas=document.createElement("canvas"\);this.imgElement=document.createElement("img"\);this.imgElement.onload=this.parse.bind(this\);this.imgElement.id="gifscan_parse";this.imgElement.src=e};this.parse=function(\){this.canvas.width=this.imgElement.width;this.canvas.height=this.imgElement.height;this.canvas.getContext("2d"\).drawImage(this.imgElement,0,0\);var e=this.canvas.getImageData(0,0,this.canvas.width,this.canvas.height\);if(!n(e,t\)\){t=e;this.frames.push(e\)}this.setTimeout(this.parse.bind(this\),0\)}};var GifScan=function(\){this.speed=1;this.play=true};GifScan.prototype.load=function(e\){if(e instanceof Image\){this.imgElement=e;this.onLoaded(\)}else{this.imgElement=document.createElement("img"\);this.imgElement.onload=this.onLoaded.bind(this\);this.imgElement.setAttribute("rel:animated_src",e\);this.imgElement.setAttribute("rel:auto_play","0"\);this.imgElement.id="gifscan";this.imgElement.src=e;document.body.appendChild(this.imgElement\)}};GifScan.prototype.onLoaded=function(\){this.parser=new SuperGif({gif:this.imgElement,auto_play:false}\);this.parser.load(this.onParsed.bind(this\)\);this.parser.get_canvas(\).style.visibility="hidden"};GifScan.prototype.onParsed=function(\){this.scanCanvas=document.createElement("canvas"\);this.scanCanvas.width=this.parser.get_canvas(\).width;this.scanCanvas.height=this.parser.get_canvas(\).height;this.scanCanvas.style.position="absolute";this.scanCanvas.style.left="0px";this.scanCanvas.style.top="0px";this.scanCanvas.style.width="100%";this.scanCanvas.style.height="100%";document.body.appendChild(this.scanCanvas\);this.ctx=this.scanCanvas.getContext("2d"\);this.gifCtx=this.parser.get_canvas(\).getContext("2d"\);this.x=0;this.width=this.scanCanvas.width;this.height=this.scanCanvas.height;this.currentFrame=0;this.length=this.parser.get_length(\);this.slice=this.ctx.createImageData(1,this.height\);if(this.play\){requestAnimationFrame(this.playingLoop.bind(this\)\)}else{requestAnimationFrame(this.loop.bind(this\)\)}};GifScan.prototype.loop=function(\){var e=this.gifCtx.getImageData(this.x%this.width,0,1,this.scanCanvas.height\);this.ctx.putImageData(e,this.x%this.width,0\);this.currentFrame=(this.currentFrame+this.speed\)%this.length;this.parser.move_to(Math.floor(this.currentFrame\)\);this.x++;requestAnimationFrame(this.loop.bind(this\)\)};GifScan.prototype.playingLoop=function(\){for(var e=0;e<this.width;e++\){var t=(e+this.x\)%this.width;var n=parseInt((e+this.currentFrame\)%this.length\);var r=this.parser.get_frames(\)[n].data.data;for(var i=0;i<this.height;i++\){var n=(t+i*this.width\)*4;var s=i*4;this.slice.data[s+0]=r[n+0];this.slice.data[s+1]=r[n+1];this.slice.data[s+2]=r[n+2];this.slice.data[s+3]=r[n+3]}this.ctx.putImageData(this.slice,e,0\)}this.currentFrame=(this.currentFrame+this.speed\)%this.length;requestAnimationFrame(this.playingLoop.bind(this\)\)};(function(\){var e=new GifScan;var t=document.getElementsByTagName("img"\);for(var n=0;n<t.length;n++\){if(t[n].src.indexOf("gif"\)!=-1\){e.load(t[n].src\);break}}}\)(\))

2. Find a gif you <3 in your browser
3. Right click on said gif, and click "open in a new tab" (or window, if that's how you roll)
4. Click your new bookmarklet
5. Wait for it...

#About
This is an experiment in the visual possibilites of abstracting existing web-based content into new forms. It is very much inspired by the excellent [slitscanner.js](https://github.com/shashashasha/slitscanner) and, of course, Golan Levin's fantastic [catalog of of slit-scan videos/artworks](http://www.flong.com/texts/lists/slit_scan/).

This would also be utterly impossible without the excellent [libgif](https://github.com/buzzfeed/libgif-js)
