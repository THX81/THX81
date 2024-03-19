JS MD5 Benchmark
================

Hello, just a quick one. I was recently looking at ways to improve our company website's MD5 JavaScript implementation. I selected three of the most common JS libraries from the NPM website and included one from Joseph Myers for comparison.

The testing code isn't a highly standardized benchmark, I wanted to get a rough estimate of the relative speed of each library. You can see the results below.

Feel free to use the JS code for your own testing. Note that some libraries have different names for the MD5 function, so you'll need to comment and uncomment the appropriate includes and function calls within the loop.

We were previously using blueimp-md5, which is decent, but I've switched to Joseph's library. It's half the size and potentially 20% faster.

Cheers 👋

```html
<!--<script src="~/Scripts/node_modules/crypto-js/crypto-js.js"></script><script src="~/Scripts/node_modules/crypto-js/md5.js"></script>-->
<!-- Last avg hashes/s: 251058.25 -->

<!--<script src="~/Scripts/node_modules/md5/dist/md5.min.js"></script>-->
<!-- Last avg hashes/s: 475286.75 -->

<!--<script src="~/Scripts/node_modules/blueimp-md5/js/md5.js"></script>-->
<!-- Last avg hashes/s: 595135 -->

<script src="~/Scripts/Modules/md5.js"></script>
<!-- https://www.myersdaily.org/joseph/javascript/md5-text.html -->
<!-- Last avg hashes/s: 798421.75 -->

<p>Template: 8c7c345ce2a67662e02244778dae2f59</p>

<button id="btnstart">start</button>
<button id="btnstop">stop</button>
<p id="jsprogress"></p>
<p id="jsresult"></p>

<script type="text/javascript">
    const btnstart = document.getElementById("btnstart");
    const btnstop = document.getElementById("btnstop");
    const jsprogress = document.getElementById("jsprogress");
    const jsResult = document.getElementById("jsresult");
    let shouldStop = false;
    const oneTestSec = 2;
    const repeateCount = 2;

    function test() {
        shouldStop = false;
        jsprogress.innerHTML = "";
        let tests = 0;
        let totalCPS = 0;

        for (let x = 0; x < repeateCount; x++) {
            if (shouldStop)
                break;
            jsprogress.innerHTML += ". ";

            let cycles = 0;
            let startTime = new Date().getTime();
            let md5_result;
            while (startTime + (oneTestSec * 1000) > new Date().getTime()) {
                //md5_result = CryptoJS.MD5("Mediterranean");//crypto-js
                //md5_result = MD5("Mediterranean");//md5
                md5_result = md5("Mediterranean");//blueimp-md5 & md5.js

                cycles++;
            }
            let cps = cycles / oneTestSec;
            jsResult.innerHTML += `
            Result: ${md5_result}<br />
            Cycles: ${cycles}<br />
            <strong>CPS</strong>: ${cps}<br />
            <br />`;

            tests++;
            totalCPS += cps;
        }

        jsResult.innerHTML += `
        tests: ${tests}<br />
        TOT CPS: ${totalCPS}<br />
        <strong>AVG CPS</strong>: ${totalCPS / tests}<br />
        <br />`;

        btnstart.removeAttribute("disabled");
    }

    btnstart.addEventListener("click", function () {
        btnstart.setAttribute("disabled", "disabled");
        setTimeout(test, 100);
    });
    btnstop.addEventListener("click", function () {
        shouldStop = true;
    });

</script>

```