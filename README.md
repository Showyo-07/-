# -<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>Irish Tunes Randomizer</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
  body {
    font-family: "Helvetica Neue", "Arial", sans-serif;
    margin: 0;
    padding: 0;
    background: #f2f3f7;
    color: #333;
  }

  header {
    background: linear-gradient(135deg, #4CAF50, #2e7d32);
    color: white;
    padding: 22px;
    text-align: center;
    font-size: 1.6em;
    font-weight: bold;
    letter-spacing: 1px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.25);
  }

  .container {
    padding: 20px;
    max-width: 760px;
    margin: auto;
  }

  button {
    width: 100%;
    padding: 16px;
    margin: 12px 0;
    font-size: 1.2em;
    border: none;
    border-radius: 12px;
    background: #4CAF50;
    color: white;
    font-weight: bold;
    box-shadow: 0 3px 8px rgba(0,0,0,0.2);
    transition: 0.2s;
  }

  button:hover {
    background: #43a047;
    transform: translateY(-2px);
  }

  #result {
    margin-top: 20px;
    padding: 18px;
    background: white;
    border-radius: 12px;
    font-size: 1.4em;
    font-weight: bold;
    text-align: center;
    box-shadow: 0 3px 10px rgba(0,0,0,0.15);
  }

  #youtubeLink, #sessionLink {
    margin-top: 12px;
    text-align: center;
    font-size: 1.15em;
  }

  .fav-btn {
    margin-top: 14px;
    padding: 12px;
    background: #ff9800;
    border-radius: 10px;
    color: white;
    font-weight: bold;
    cursor: pointer;
    text-align: center;
    box-shadow: 0 3px 8px rgba(0,0,0,0.2);
  }

  h2 {
    margin-top: 40px;
    border-left: 6px solid #4CAF50;
    padding-left: 12px;
    font-size: 1.3em;
  }

  .list-block {
    background: white;
    padding: 18px;
    margin: 14px 0 30px 0;
    border-radius: 12px;
    box-shadow: 0 3px 10px rgba(0,0,0,0.15);
  }

  .tune-item {
    padding: 6px 0;
    border-bottom: 1px solid #eee;
    font-size: 1.05em;
  }

  .tune-item:last-child {
    border-bottom: none;
  }

  #searchBox {
    width: 100%;
    padding: 14px;
    font-size: 1.15em;
    border-radius: 10px;
    border: 1px solid #ccc;
    margin-top: 20px;
    margin-bottom: 12px;
  }

  .small-btn {
    background: #e53935;
    color: white;
    padding: 4px 10px;
    border-radius: 6px;
    margin-left: 10px;
    cursor: pointer;
    font-size: 0.8em;
  }

  .clickable {
    color: #1e88e5;
    cursor: pointer;
  }
</style>
</head>

<body>

<header>Irish Tunes Randomizer（Reel / Jig / Polka）</header>

<div class="container">

  <button id="randomBtn">🎵 ランダムに1曲表示</button>
  <button id="countBtn">📊 曲数を表示</button>

  <div id="result">ここに結果が表示されます</div>
  <div id="youtubeLink"></div>
  <div id="sessionLink"></div>
  <div id="favButton"></div>

  <h2>⭐ お気に入り（Favorite）</h2>
  <div id="favoriteList" class="list-block"></div>

  <h2>🕒 最近選んだ曲（History）</h2>
  <div id="historyList" class="list-block"></div>

  <h2>🔍 曲検索（全ジャンル横断）</h2>
  <input id="searchBox" type="text" placeholder="曲名を入力すると絞り込みできます">
  <div id="searchResults" class="list-block"></div>

  <h2>① Reel（リール）</h2>
  <div id="list1" class="list-block"></div>

  <h2>② Jig（ジグ）</h2>
  <div id="list2" class="list-block"></div>

  <h2>③ Polka（ポルカ）</h2>
  <div id="list3" class="list-block"></div>

  <h2>📚 全曲まとめ一覧（Reel / Jig / Polka）</h2>
  <div id="allList" class="list-block"></div>

</div>

<script>
/* ---------------------------------------------------------
   ★ Reel / Jig / Polka の全曲リスト（クリーン版）
--------------------------------------------------------- */

const list1 = [
"Anderson’s","Bank of Ireland","Banshee","Bird in the Bush","Boyne Hunt","Boys of ’45",
"Boys of Malin","Bucks of Oranmore","Bunker Hill","Caher Rua","Cameronian","Canon Reel",
"Castle Kelly","Celbridge","Chicago reel","Christmas Eve","Clougher reel","College Grove",
"Come Up to the Room, I Want Ye","Come West Along the Road","Concertina Reel",
"Connemara Stocking","Cooley’s","Cregg’s Pipe","Cronin’s","Cup of Tea","Derry",
"Devanney’s Goat","Dick Sherlock’s","Donegal","Doon","Dr. Gilbert","Drowsy Maggie",
"Duke of Leinster","Earl’s Chair","Fair Winds","Farewell to Erin","Father Kelly’s",
"Fox on the Prowl","Foxhunter","Frank’s","Fred Finn's","Galtee Rangers","Galway Rambler",
"George White’s Favorite","Glass of Beer","Gravel Walks","Green Fields of America",
"Green Mountain","Hare’s Paw","High Drive","Humours of Ballyconnell","Hunter’s House",
"Jackie Coleman’s","Jenny Picking Cockles","Jenny’s Chicken","Jenny’s Wedding",
"Jim McCormick’s","Jimmy Kelly’s","Jimmy’s Return","John Brennan’s","Kerry","Kilmaley",
"Killarney Boys of Pleasure","Kitty Gone a-Milking","Lad O’Beirne’s",
"Lady Ann Montgomery","Lady on the Island","Last Night’s Fun","Little Bag of Spuds",
"Lochaber Badger","London Lasses","Long Note","Longford Collector","Lucky in Love",
"MacArthur Road","Maid Behind the Bar","Maid of Mt. Kisco","Martin Wynne’s No.1",
"Martin Wynne’s No.2","Martin Wynne’s No.3","Maud Miller","Merry Blacksmith",
"Miss McLeod","Miss Monaghan","Morning Star","Mountain Road","Musical Priest",
"Music for a Found Harmonium","My Love in America","Nervous Man","New Copperplate",
"New Policeman","Noel Hill’s","Old Blackthorn","Old Bush","Old Copperplate",
"Over the Moor to Maggie","Peeler’s Jacket","Pelican Marsh","Pigeon on the Gate",
"Put in Me the Big Chest","Red-Haired Lass","Rip the Calico","Road to Rio",
"Roaring Mary","Sailor on the Rock","Sailor’s Bonnet","Salamanca","Sally Gardens",
"Sean McGuire’s","Sean Reid’s","Shannon Breeze","Shaskeen","Sheehan’s",
"Ships Are Sailing","Shoemaker’s Daughter","Silver Spear","Silver Spire","Skylark",
"Sligo Maid","St. Ann’s","Sunny Banks","Swallowtail","Swinging on the Gate",
"Tarbolton","Teetotaler","Tim Maloney’s","Tinker’s Daughter","Tom Ward’s Downfall",
"Tommy People’s","Traver’s","Trip to Durrow","Tulla","Wind that Shakes the Barley",
"Wise Maid","Woman of the House"
];

const list2 = [
"Bag Of Beer","Battering Ram","Behind The Haystack","Bill Collin’s","Blarney Pilgrim",
"Boys Of The Town","Bush On The Hill","Carraroe","Christy Barry’s 1","Christy Barry’s 2",
"Cliffs Of Moher","Club Ceili","Cobbler","Cock In The Kitchen","Connachtman’s Rambles",
"Cuil Aodha jig","Don’t Touch That Green Linnet","Dusty Windmills","Eavesdropper",
"Father O’Flynn","Feed The Ducks","Flowers Of Burren","Frost Is All Over",
"Garrett Bally’s","Geese In The Bog","Gillian’s Apple","Girls Of Grallagh",
"Haunted House","Haste To The Wedding","Health Of The Ladies","Hole In The Bridge",
"How About Whiskey Instead","Humours Of Drinagh","I Ne’er Shall Wean Her",
"Irish Washerman","Jackson’s Morning Breeze","Jerry’s Beaver Hat","Jim Ward’s",
"Joe Cooley’s","Joe Derrane’s","Jump At The Sun","Kerfunten","Kerry","Kesh",
"Kilavil","Kilfenora 2","Kilfenora 3","Kilmovee","Kevin McHugh’s","Lad O’Beirn’s",
"Langstrom’s Pony","Lark In The Morning","Lark On The Strand","Leitrim",
"Lilting Banshees","Lilting Fisherman","Longnancy’s","Luck Penny",
"Maid At The Spinning Wheel","Maid In The Meadow","Maid On The Green",
"Michael Hynes’","Monaghan","Morning Lark","Mouse In The Mug","My Darling Asleep",
"North Clare","Old John’s","Out On The Ocean","Paddy Fahey’s 1","Pay The Reckoning",
"Pipe On The Hob 1","Pipe On The Hob 2","Queen Of The Fair","Rambler",
"Rambling Pitchfork","Road To Rinehisson","Rose In The Heather","Sackow’s",
"Saddle The Pony","Shandon Bells","Ships In Full Sail","Sixpenny Money",
"Steven Campbell","Strike The Gay Harp","Tar Trip To Sligo","Tell Her I Am",
"Timmy Clifford’s","Tobin’s Favorite","Tom Billy’s","Tongs By The Fire",
"Trip To Athlone","Wandering Minstrel","When You Sick, Is It Tae You Want?",
"Whelan’s Old Sow","Willie Coleman’s"
];

const list3 = [
"Away The Wattle-O","Ballydesmond1","Ballydesmond2","Ballydesmond3",
"Ballyhoura Mountains","Blackwater 1","Blackwater 2","Britches Full Of Stitches",
"Character’s","Church Street","Corner House","Cuil Aodha","Dalaigh’s","Daly's Mill",
"Denis Murphy’s","Egan’s","Forde’s","Green Cottage","Gurteen Cross",
"Happy Polka","I’ll Tell Me Ma","I’ll Buy Boots For Maggie","John Ryan’s",
"John Collin’s","John Egan’s","John Walsh’s","Last Chance","Little Diamond",
"Maggie In The Woods","Maids Of Ardagh","Many A Wild Night","Mick’s",
"Murroe Polka","Ned Kelly’s","New Market","Newmarket Polka","O’Callaghan’s",
"Pamela’s Pasta Party","Pop1","Pop2","Pop3","Rakes Of Mallow","Rattling Bog",
"Ray’s Classic","Rooskey","Scatergalen","Spanish Lady","Sweeney’s",
"Terry Teehan’s","Tolka","Tom Sullivans","Trip To Dingle","Tripping To The Well"
];

/* 全曲統合 */
const allTunes = Array.from(new Set([...list1, ...list2, ...list3]));

/* ---------------- お気に入りと履歴 ---------------- */
let favorites = [];
let history = [];

function renderFavorites() {
  const sorted = [...favorites].sort((a,b)=>a.localeCompare(b));
  document.getElementById("favoriteList").innerHTML =
    sorted.map(t => 
      `<div class="tune-item">
        ${t}
        <span class="small-btn" onclick="removeFavorite('${t}')">×</span>
      </div>`
    ).join("") || "（まだありません）";
}

function addFavorite(tune) {
  if (!favorites.includes(tune)) favorites.push(tune);
  renderFavorites();
}

function removeFavorite(tune) {
  favorites = favorites.filter(t => t !== tune);
  renderFavorites();
}

function renderHistory() {
  document.getElementById("historyList").innerHTML =
    history.map(t => 
      `<div class="tune-item clickable" onclick="showLinks('${t}')">${t}</div>`
    ).join("") || "（まだありません）";
}

function addHistory(tune) {
  history.unshift(tune);
  if (history.length > 20) history.pop();
  renderHistory();
}

/* ---------------- ランダム表示 ---------------- */
document.getElementById("randomBtn").onclick = () => {
  const idx = Math.floor(Math.random() * allTunes.length);
  const tune = allTunes[idx];

  document.getElementById("result").textContent =
    `🎵 ランダム曲： ${tune}`;

  showLinks(tune);
  addHistory(tune);

  document.getElementById("favButton").innerHTML =
    `<div class="fav-btn" onclick="addFavorite('${tune}')">★ お気に入りに追加</div>`;
};

/* ---------------- YouTube / TheSession リンク生成 ---------------- */
function showLinks(tune) {
  const ytURL = `https://www.youtube.com/results?search_query=${encodeURIComponent(tune+ " irish music")}`;
  const sessionURL = `https://thesession.org/tunes/search?search=${encodeURIComponent(tune)}`;

  document.getElementById("youtubeLink").innerHTML =
    `<a href="${ytURL}" target="_blank">▶ YouTubeで「${tune}」を検索</a>`;

  document.getElementById("sessionLink").innerHTML =
    `<a href="${sessionURL}" target="_blank">▶ TheSession.org で「${tune}」を検索</a>`;
}

/* ---------------- 曲数表示 ---------------- */
document.getElementById("countBtn").onclick = () => {
  document.getElementById("result").textContent =
    `📊 登録されている曲は全部で ${allTunes.length} 曲です`;
  document.getElementById("youtubeLink").innerHTML = "";
  document.getElementById("sessionLink").innerHTML = "";
  document.getElementById("favButton").innerHTML = "";
};

/* ---------------- アルファベット順一覧 ---------------- */
function renderList(targetId, arr) {
  const sorted = [...arr].sort((a,b)=>a.localeCompare(b));
  document.getElementById(targetId).innerHTML =
    sorted.map(t => `<div class="tune-item">${t}</div>`).join("");
}

renderList("list1", list1);
renderList("list2", list2);
renderList("list3", list3);

/* ---------------- 🔍 検索機能（クリックでリンク表示） ---------------- */
document.getElementById("searchBox").addEventListener("input", () => {
  const keyword = document.getElementById("searchBox").value.toLowerCase();
  const results = allTunes.filter(t => t.toLowerCase().includes(keyword));

  document.getElementById("searchResults").innerHTML =
    results.map(t => 
      `<div class="tune-item clickable" onclick="showLinks('${t}')">${t}</div>`
    ).join("") ||
    "<div class='tune-item'>該当なし</div>";
});

</script>

</body>
</html>
