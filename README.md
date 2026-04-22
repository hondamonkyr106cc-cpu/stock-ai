[index.html](https://github.com/user-attachments/files/26962929/index.html)
[style.css](https://github.com/user-attachments/files/26962930/style.css)[app.js](https://github.com/user-attachments/files/26962941/app.js)
[app.js](https://github.com/user-attachments/files/26962931/app.js)
const universe = [
    {code:"7203", name:"トヨタ"},
    {code:"6758", name:"ソニー"},
    {code:"9984", name:"SBG"},
    {code:"8306", name:"三菱UFJ"},
    {code:"9432", name:"NTT"},
    {code:"7014", name:"名村造船"},
    {code:"9348", name:"ispace"},
    {code:"7011", name:"三菱重工"},
    {code:"7003", name:"三井E&S"}
];

const watchList = [
    {code:"7014", name:"名村造船所"},
    {code:"9348", name:"ispace"},
    {code:"7011", name:"三菱重工業"},
    {code:"7003", name:"三井E&S"},
    {code:"", name:""}
];

function evaluate(){
    let trend = Math.random();
    let vol = Math.random();

    let score = trend*0.7 + (1-vol)*0.3;
    let exp = (trend-0.5)*0.04;

    let signal =
        score > 0.65 ? "🟢BUY" :
        score < 0.4 ? "🔴SELL" :
        "🟡WAIT";

    return {score, exp, signal};
}

function run(){

    let all=[];

    universe.forEach(s=>{
        let r = evaluate();

        all.push({
            code:s.code,
            name:s.name,
            score:r.score,
            exp:r.exp,
            signal:r.signal
        });
    });

    all.sort((a,b)=>b.exp-a.exp);

    let top5 = all.slice(0,5);

    let rhtml = "<h3>🔥 上位5ランキング</h3>";

    top5.forEach((s,i)=>{
        rhtml += `
        <div class="card">
            <div class="rank">${i+1}位 ${s.name}</div>
            <div class="code">${s.code}</div>
            <div class="good">期待値 ${(s.exp*100).toFixed(2)}%</div>
            <div class="tag">${s.signal}</div>
        </div>`;
    });

    document.getElementById("rank").innerHTML = rhtml;

    let whtml = "<h3>📌 ウォッチ</h3>";

    watchList.forEach(s=>{
        let r = evaluate();

        whtml += `
        <div class="card">
            <div class="rank">${s.name || "未設定"}</div>
            <div class="code">${s.code || "-"}</div>
            <div class="good">期待値 ${(r.exp*100).toFixed(2)}%</div>
            <div class="tag">${r.signal}</div>
        </div>`;
    });

    document.getElementById("watch").innerHTML = whtml;
}
