<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="referrer" content="strict-origin-when-cross-origin" />
    <title>초등 체육 - KBO 등장곡 플레이어 (순서변경/삭제 지원)</title>
    <style>
        /* 폰트 적용 */
        @import url('https://fonts.googleapis.com/css2?family=Black+Han+Sans&family=Do+Hyeon&display=swap');
        
        body { 
            background: #00256c; 
            color: white; 
            font-family: 'Do Hyeon', sans-serif; 
            margin: 0; padding: 20px; 
        }
        .container { max-width: 1100px; margin: 0 auto; }
        
        h1 { 
            text-align: center; 
            font-family: 'Black Han Sans', sans-serif; 
            font-size: 55px; 
            color: #ffcc00; 
            text-shadow: 4px 4px #000; 
            margin-bottom: 5px; letter-spacing: 2px;
        }
        p.subtitle { text-align: center; color: #a9c2f0; margin-top: 0; margin-bottom: 25px; font-size: 20px; }
        
        /* 탭 디자인 */
        .tab-container { display: flex; flex-wrap: wrap; gap: 5px; margin-bottom: 20px; border-bottom: 6px solid #ffcc00; padding-bottom: 0; }
        .tab-btn { 
            background: #cfd8dc; color: #00256c; border: none; padding: 12px 25px; font-size: 22px; 
            border-radius: 15px 15px 0 0; cursor: pointer; font-family: 'Black Han Sans', sans-serif; transition: 0.2s; 
        }
        .tab-btn:hover { background: #b0bec5; }
        .tab-btn.active { background: #ffcc00; color: #00256c; }
        .delete-tab { margin-left: 10px; color: #d32f2f; background: none; border: none; cursor: pointer; font-size: 18px; padding: 0; }
        .add-tab-btn { background: #ff5252; color: white; border-radius: 15px 15px 0 0; }
        
        /* 반 추가 입력창 */
        .setup-box { background: #ffcc00; color: #00256c; padding: 20px; border-radius: 15px; margin-bottom: 25px; display: none; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
        .setup-box h3 { font-family: 'Black Han Sans', sans-serif; font-size: 26px; margin-top: 0; margin-bottom: 10px; }
        .setup-box input { padding: 12px; border: 3px solid #00256c; border-radius: 10px; font-size: 18px; margin-right: 5px; margin-bottom: 5px; font-family: 'Do Hyeon', sans-serif;}
        .setup-box button { padding: 12px 20px; font-size: 20px; background: #00256c; color:#ffcc00; border:none; border-radius:10px; cursor:pointer; font-family: 'Black Han Sans', sans-serif; }
        
        /* 상단 컨트롤 바 */
        .top-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; background: rgba(255,255,255,0.1); padding: 15px 20px; border-radius: 15px; }
        .top-bar h2 { font-family: 'Black Han Sans', sans-serif; font-size: 34px; margin: 0; color: #ffcc00; text-shadow: 2px 2px #000; display: inline-block; }
        #sync-status { font-size: 18px; color: #90caf9; margin-left: 15px; }
        
        button.stop-btn { background: #d32f2f; color:white; padding: 12px 25px; font-size: 20px; border:none; border-radius:10px; cursor:pointer; font-family: 'Black Han Sans', sans-serif; }
        button.sync-btn { background: #1976d2; color:white; padding: 12px 25px; font-size: 20px; border:none; border-radius:10px; cursor:pointer; font-family: 'Black Han Sans', sans-serif; margin-right: 10px; }
        
        /* 선수 카드 그리드 */
        .player-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; }
        .student-card { background: white; color: #00256c; border-radius: 20px; padding: 20px; text-align: center; border-bottom: 10px solid #ffcc00; position: relative; box-shadow: 0 5px 15px rgba(0,0,0,0.4); }
        .student-card.playing { border-bottom-color: #d32f2f; background-color: #ffebee; animation: pulse 1.5s infinite; }
        
        /* 이름 및 뱃지 */
        .name-row { display: flex; align-items: center; justify-content: flex-start; margin-bottom: 10px; position: relative; padding-right: 110px; }
        .name { font-size: 36px; font-family: 'Black Han Sans', sans-serif; color: #00256c; }
        .num-badge { background: #00256c; color: #ffcc00; font-size: 18px; font-family: 'Black Han Sans', sans-serif; padding: 6px 12px; border-radius: 12px; margin-right: 10px; }
        
        /* 카드 우측 상단 관리 버튼 (순서/수정/삭제) */
        .card-controls { position: absolute; right: 0; top: 0; display: flex; gap: 3px; }
        .control-btn { background: #f0f0f0; border: 1px solid #ccc; border-radius: 6px; padding: 5px; font-size: 16px; cursor: pointer; transition: 0.2s; }
        .control-btn:hover { background: #e0e0e0; transform: scale(1.1); }
        .delete-btn { background: #ffebee; border-color: #ffcdd2; }
        .delete-btn:hover { background: #ffcdd2; }
        
        /* 구호 및 버튼 */
        .cheer { font-size: 24px; color: #d32f2f; background: #f5f5f5; padding: 12px; border-radius: 12px; margin-bottom: 15px; word-break: keep-all; min-height: 30px; line-height: 1.3; text-align: center;}
        .play-btn { background: #00256c; color:#ffcc00; border:none; width: 100%; padding: 15px; font-size: 24px; border-radius: 50px; cursor:pointer; font-family: 'Black Han Sans', sans-serif; letter-spacing: 2px; transition: 0.2s;}
        .play-btn:hover { background: #001540; }
        .student-card.playing .play-btn { background: #d32f2f; color: white; }
        
        /* 수정 폼 */
        .edit-form { display: none; flex-direction: column; gap: 8px; text-align: left; background: #e0e0e0; padding: 15px; border-radius: 15px; margin-top: 15px; }
        .edit-form input { padding: 10px; border: none; border-radius: 8px; font-family: 'Do Hyeon', sans-serif; font-size: 18px; color: #00256c; }
        .edit-btns { display: flex; gap: 5px; margin-top: 5px; }
        .edit-btns button { flex: 1; padding: 12px; font-size: 20px; border:none; border-radius:8px; cursor:pointer; font-family: 'Black Han Sans', sans-serif; color: white; }
        
        #youtube-box { margin-top: 40px; text-align: center; padding: 20px; background: rgba(0,0,0,0.6); border-radius: 15px; display: none; }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(211, 47, 47, 0.4); } 70% { box-shadow: 0 0 0 15px rgba(211, 47, 47, 0); } }
    </style>
</head>
<body>

<div class="container">
    <h1>KBO ENTRY PLAYER</h1>
    <p class="subtitle">made by 소정</p>

    <div class="tab-container" id="tab-container"></div>

    <div class="setup-box" id="add-class-box">
        <h3>➕ 새로운 반 엔트리 등록</h3>
        <input type="text" id="new-class-name" placeholder="반 이름 (예: 1반)" style="width: 140px;">
        <input type="text" id="new-class-url" placeholder="구글 시트 CSV 링크 붙여넣기" style="width: calc(100% - 290px); min-width:200px;">
        <button onclick="addClass()">저장하기</button>
        <button onclick="toggleAddBox()" style="background:#fff; color:#d32f2f;">취소</button>
    </div>

    <div id="dashboard" style="display: none;">
        <div class="top-bar">
            <div>
                <h2 id="current-class-title">라인업</h2>
                <span id="sync-status"></span>
            </div>
            <div>
                <button class="sync-btn" onclick="forceSync()">🔄 시트 최신화</button>
                <button class="stop-btn" onclick="stopAll()">⏹️ 전체 정지</button>
            </div>
        </div>
        <div class="player-grid" id="cards-container"></div>
    </div>
    
    <div id="youtube-box"></div>
</div>

<script>
    var currentPlayingId = null;
    var stopTimeout = null;
    
    var classes = JSON.parse(localStorage.getItem('baseballClasses')) || [];
    var localEdits = JSON.parse(localStorage.getItem('baseballLocalEdits')) || {};
    var activeClassIndex = localStorage.getItem('baseballActiveIndex') || 0;
    var studentsData = [];

    window.onload = function() {
        renderTabs();
        if (classes.length > 0) {
            selectClass(activeClassIndex >= classes.length ? 0 : activeClassIndex);
        } else {
            toggleAddBox();
        }
    };

    function getYoutubeId(url) {
        if(!url) return '';
        var regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
        var match = url.match(regExp);
        return (match && match[2].length == 11) ? match[2] : '';
    }

    function parseCSV(text) {
        var p = '', row = [''], ret = [row], i = 0, r = 0, s = !0, l;
        for (l of text) {
            if ('"' === l) {
                if (s && l === p) row[i] += l;
                s = !s;
            } else if (',' === l && s) l = row[++i] = '';
            else if ('\n' === l && s) {
                if ('\r' === p) row[i] = row[i].slice(0, -1);
                row = ret[++r] = [l = '']; i = 0;
            } else row[i] += l;
            p = l;
        }
        return ret.filter(r => r.length > 1 && r[0].trim() !== ''); 
    }

    function renderTabs() {
        var container = document.getElementById('tab-container');
        container.innerHTML = '';
        
        classes.forEach((cls, idx) => {
            var btn = document.createElement('button');
            btn.className = 'tab-btn' + (idx == activeClassIndex ? ' active' : '');
            btn.innerHTML = `${cls.name} <span class="delete-tab" onclick="event.stopPropagation(); deleteClass(${idx})" title="삭제">✖</span>`;
            btn.onclick = () => selectClass(idx);
            container.appendChild(btn);
        });

        var addBtn = document.createElement('button');
        addBtn.className = 'tab-btn add-tab-btn';
        addBtn.innerText = '➕ 반 추가';
        addBtn.onclick = toggleAddBox;
        container.appendChild(addBtn);
    }

    function toggleAddBox() {
        var box = document.getElementById('add-class-box');
        box.style.display = box.style.display === 'block' ? 'none' : 'block';
    }

    function addClass() {
        var name = document.getElementById('new-class-name').value.trim();
        var url = document.getElementById('new-class-url').value.trim();
        if(!name || !url) { alert("반 이름과 시트 링크를 모두 입력해주세요."); return; }
        
        classes.push({ name: name, url: url });
        localStorage.setItem('baseballClasses', JSON.stringify(classes));
        
        document.getElementById('new-class-name').value = '';
        document.getElementById('new-class-url').value = '';
        toggleAddBox();
        
        selectClass(classes.length - 1);
    }

    function deleteClass(idx) {
        if(confirm(`'${classes[idx].name}' 탭을 삭제하시겠습니까?`)) {
            var deletedName = classes[idx].name;
            classes.splice(idx, 1);
            delete localEdits[deletedName]; 
            
            localStorage.setItem('baseballClasses', JSON.stringify(classes));
            localStorage.setItem('baseballLocalEdits', JSON.stringify(localEdits));
            
            if(classes.length === 0) {
                document.getElementById('dashboard').style.display = 'none';
                activeClassIndex = 0;
            } else {
                selectClass(0);
            }
            renderTabs();
        }
    }

    function selectClass(idx) {
        if(classes.length === 0) return;
        activeClassIndex = idx;
        localStorage.setItem('baseballActiveIndex', activeClassIndex);
        renderTabs();
        
        var currentClass = classes[idx];
        document.getElementById('current-class-title').innerText = currentClass.name + ' ENTRY';
        
        if(localEdits[currentClass.name]) {
            studentsData = localEdits[currentClass.name];
            document.getElementById('sync-status').innerText = "(로컬 기기 편집됨 ✏️)";
            renderDashboard();
        } else {
            fetchCSV(currentClass);
        }
    }

    function forceSync() {
        if(classes.length === 0) return;
        var currentClass = classes[activeClassIndex];
        if(confirm(`구글 시트의 원본 데이터를 다시 불러오시겠습니까?\n(웹에서 직접 추가/삭제/수정한 내용은 모두 사라집니다)`)) {
            delete localEdits[currentClass.name];
            localStorage.setItem('baseballLocalEdits', JSON.stringify(localEdits));
            fetchCSV(currentClass);
        }
    }

    async function fetchCSV(classObj) {
        document.getElementById('sync-status').innerText = "(시트 동기화 중...)";
        try {
            var separator = classObj.url.indexOf('?') === -1 ? '?' : '&';
            var bypassCacheUrl = classObj.url + separator + 'nocache=' + new Date().getTime();

            let response = await fetch(bypassCacheUrl, { cache: 'no-store' });
            let data = await response.text();
            
            var rows = parseCSV(data);
            studentsData = [];
            
            // 데이터 고유 ID 부여를 위한 시간 변수
            var timeStamp = new Date().getTime();
            
            for(var idx = 1; idx < rows.length; idx++) {
                var cols = rows[idx];
                if(cols.length >= 6) {
                    var num = cols[0].trim();
                    var name = cols[1].trim();
                    var cheer = cols[2].trim();
                    var ytUrl = cols[3].trim();
                    var startSec = cols[4] ? parseInt(cols[4].trim()) : 0;
                    var endSec = cols[5] ? parseInt(cols[5].trim()) : (startSec + 10);
                    var ytId = getYoutubeId(ytUrl);

                    if(name) {
                        // 순서 변경/삭제를 위해 배열 인덱스가 아닌 고유 ID(timeStamp + idx) 사용
                        studentsData.push({ id: timeStamp + idx, num: num, name: name, cheer: cheer, ytUrl: ytUrl, ytId: ytId, start: startSec, end: endSec });
                    }
                }
            }
            saveLocalData();
            document.getElementById('sync-status').innerText = "(시트 동기화 완료 ✅)";

        } catch (error) {
            alert('데이터를 불러오지 못했습니다. 게시된 주소가 맞는지 확인해주세요.');
            document.getElementById('sync-status').innerText = "(동기화 실패 ❌)";
        }
    }

    // 데이터를 로컬(브라우저)에 저장하고 화면을 다시 그리는 공통 함수
    function saveLocalData() {
        var currentClass = classes[activeClassIndex].name;
        localEdits[currentClass] = studentsData;
        localStorage.setItem('baseballLocalEdits', JSON.stringify(localEdits));
        renderDashboard();
    }

    // 명단 위/아래 순서 변경 기능
    function moveStudent(id, direction) {
        var index = studentsData.findIndex(s => s.id === id);
        if (index < 0) return;
        var newIndex = index + direction;
        
        // 배열 범위를 벗어나지 않도록 방어
        if (newIndex < 0 || newIndex >= studentsData.length) return;
        
        // 자리 바꾸기
        var temp = studentsData[index];
        studentsData[index] = studentsData[newIndex];
        studentsData[newIndex] = temp;
        
        document.getElementById('sync-status').innerText = "(로컬 기기 편집됨 ✏️)";
        saveLocalData();
    }

    // 명단 삭제 기능
    function deleteStudent(id) {
        var student = studentsData.find(s => s.id === id);
        if(confirm(`[${student.num}] ${student.name} 선수를 명단에서 삭제하시겠습니까?`)) {
            studentsData = studentsData.filter(s => s.id !== id);
            document.getElementById('sync-status').innerText = "(로컬 기기 편집됨 ✏️)";
            saveLocalData();
        }
    }

    function renderDashboard() {
        var container = document.getElementById('cards-container');
        container.innerHTML = '';

        studentsData.forEach(function(student) {
            var card = document.createElement('div');
            card.className = 'student-card';
            card.id = 'card-' + student.id;
            
            card.innerHTML = `
                <div class="name-row">
                    <span class="num-badge">${student.num}</span> 
                    <span class="name">${student.name}</span>
                    
                    <div class="card-controls">
                        <button class="control-btn" onclick="moveStudent(${student.id}, -1)" title="위로 이동">🔼</button>
                        <button class="control-btn" onclick="moveStudent(${student.id}, 1)" title="아래로 이동">🔽</button>
                        <button class="control-btn" onclick="toggleEdit(${student.id})" title="내용 수정">✏️</button>
                        <button class="control-btn delete-btn" onclick="deleteStudent(${student.id})" title="삭제">❌</button>
                    </div>
                </div>
                <div class="cheer">${student.cheer ? student.cheer : '구호 없음'}</div>
                <button class="play-btn" id="btn-${student.id}" onclick="togglePlay(${student.id})">▶ 등장곡 재생</button>
                
                <div class="edit-form" id="edit-form-${student.id}">
                    <input type="text" id="edit-num-${student.id}" value="${student.num}" placeholder="번호">
                    <input type="text" id="edit-name-${student.id}" value="${student.name}" placeholder="이름">
                    <input type="text" id="edit-cheer-${student.id}" value="${student.cheer}" placeholder="구호">
                    <input type="text" id="edit-url-${student.id}" value="${student.ytUrl}" placeholder="유튜브 링크">
                    <div style="display:flex; gap:5px;">
                        <input type="number" id="edit-start-${student.id}" value="${student.start}" placeholder="시작(초)">
                        <input type="number" id="edit-end-${student.id}" value="${student.end}" placeholder="끝(초)">
                    </div>
                    <div class="edit-btns">
                        <button style="background:#4CAF50;" onclick="saveEdit(${student.id})">저장</button>
                        <button style="background:#9e9e9e;" onclick="toggleEdit(${student.id})">취소</button>
                    </div>
                </div>
            `;
            container.appendChild(card);
        });

        document.getElementById('dashboard').style.display = 'block';
    }

    function toggleEdit(id) {
        var form = document.getElementById('edit-form-' + id);
        form.style.display = form.style.display === 'flex' ? 'none' : 'flex';
    }

    function saveEdit(id) {
        var student = studentsData.find(s => s.id === id);
        if(student) {
            student.num = document.getElementById('edit-num-' + id).value.trim();
            student.name = document.getElementById('edit-name-' + id).value.trim();
            student.cheer = document.getElementById('edit-cheer-' + id).value.trim();
            var newUrl = document.getElementById('edit-url-' + id).value.trim();
            student.ytUrl = newUrl;
            student.ytId = getYoutubeId(newUrl) || student.ytId;
            student.start = parseInt(document.getElementById('edit-start-' + id).value) || 0;
            student.end = parseInt(document.getElementById('edit-end-' + id).value) || 0;
            
            document.getElementById('sync-status').innerText = "(로컬 기기 편집됨 ✏️)";
            saveLocalData();
        }
    }

    function togglePlay(id) {
        if (currentPlayingId === id) { stopAll(); return; }
        stopAll();
        
        var student = studentsData.find(s => s.id === id);
        if(!student || !student.ytId) { alert("유튜브 링크가 올바르지 않습니다."); return; }

        currentPlayingId = id;
        document.getElementById('card-' + id).classList.add('playing');
        document.getElementById('btn-' + id).innerText = '⏹ 정지';

        var iframeHtml = `<p style="font-size: 16px; font-family: 'Do Hyeon'; margin-top: 0;">📺 ${student.name} 재생 중</p>
                          <iframe width="300" height="169" 
                          src="https://www.youtube.com/embed/${student.ytId}?start=${student.start}&end=${student.end}&autoplay=1&origin=https://app.netlify.com" 
                          title="YouTube video player" frameborder="0" 
                          referrerpolicy="strict-origin-when-cross-origin"
                          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                          allowfullscreen></iframe>`;
        
        var ytBox = document.getElementById('youtube-box');
        ytBox.innerHTML = iframeHtml;
        ytBox.style.display = 'block';

        var duration = (student.end - student.start) * 1000;
        if(duration > 0) {
            stopTimeout = setTimeout(function() { stopAll(); }, duration);
        }
    }

    function stopAll() {
        if(stopTimeout) { clearTimeout(stopTimeout); }
        document.getElementById('youtube-box').innerHTML = ''; 
        document.getElementById('youtube-box').style.display = 'none';
        
        if(currentPlayingId !== null) {
            var card = document.getElementById('card-' + currentPlayingId);
            var btn = document.getElementById('btn-' + currentPlayingId);
            if(card) card.classList.remove('playing');
            if(btn) btn.innerText = '▶ 등장곡 재생';
        }
        currentPlayingId = null;
    }
</script>
</body>
</html>
