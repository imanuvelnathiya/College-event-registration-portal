<header>
<div class="brand"><span class="logo"aria-hidden="true"> </span>CampusEvents</div>
<nav role="tablist"aria-label="Sections">
<button role="tab"id="t-ev"aria-selected="true">Browseevents</button>
<button role="tab"id="t-my"aria-selected="false">My registrations<span class="n"id="mycount"hidden>0</span></button>
</nav>
</header>
<section id="v-ev">
<div class="tools">
<inputclass="s"id="q"type="search"placeholder="Search eventsby nameorvenue"aria-label="Search events">
<selectid="cat"aria-label="Category"></select>
<selectid="range"aria-label="Daterange">
<option value="all">Any date</option><option value="7">Next7 days</option><option value="30">Next30 days</option><option value="60">Next60
days</option>
</select>
<inputid="day"type="date"aria-label="Specificdate">
<button class="btn ghost"id="reset">Clear</button>
</div>
<div class="chips"id="chips"role="group"aria-label="Quickcategory filter"></div>
<div class="grid"id="grid"aria-live="polite"></div>
...
<dialog id="dlg"aria-labelledby="dt"></dialog>:root{color-scheme:dark;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);
--bg:#0d1020;--s1:#151930;--s2:#1c2140;--line:#2a3058;--tx:#ebeaf6;--mu:#9a9cbc;--ac:#a89bff;--ac2:#ffb48a;--ok:#6fd3a5;--bad:#ff7a8a;
.card{background:var(--s1);border:1px solid var(--line);border-radius:var(--r);padding:18px;display:flex;gap:16px;cursor:pointer;transition:border-color.
15s,transform .15s}
.seats{height:5px;border-radius:9px;background:var(--s2);overflow:hidden;margin:10px 0 6px}
.seatsdiv{height:100%;background:var(--ac);border-radius:9px}
.seats.low div{background:var(--ac2)}.seats.fulldiv{background:var(--bad)}function store(k,v){try{if(v===undefined){varr=localStorage.getItem(k);return r?JSON.parse(r):null}localStorage.setItem(k,JSON.stringify(v))}catch(e)
{return null}}
function count(id){return events.find(function(e){return e.id===id}).base+regs.filter(function(r){return r.eventId===id}).length}
function canCancel(e){return e.cancel&&startMs(e)-Date.now()>24*3600*1000}
function filtered(){
varq=$('#q').value.trim().toLowerCase(),r=$('#range').value,d=$('#day').value,t0=iso(0);
return events.filter(function(e){
if(state.cat!=='All'&&e.category!==state.cat)return false;
if(q&&(e.title+' '+e.venue+' '+e.category).toLowerCase().indexOf(q)<0)return false;
if(d&&e.date!==d)return false;
if(r!=='all'&&!(e.date>=t0&&e.date<=iso(+r)))return false;
return true}).sort(function(a,b){return a.date<b.date?-1:a.date>b.date?1:0});
function submit(e,f){
varv={name:$('#f-name').value.trim(),email:$('#f-email').value.trim().toLowerCase(),roll:$('#f-roll').value.trim(),phone:$('#f-phone').value.trim(),dept:$('#f-
dept').value,year:$('#f-year').value};
varer={};
if(!v.name)er.name='Enteryourfullname.';elseif(v.name.length<3)er.name='Namelookstoo short.';
if(!v.email)er.email='Enteryouremailaddress.';elseif(!/^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/.test(v.email))er.email='Enteravalid email, likename@college.edu.';
if(!v.roll)er.roll='Enteryourrollnumber.';
if(!v.phone)er.phone='Enteryourphonenumber.';elseif(!/^[6-9]\d{9}$/.test(v.phone.replace(/[\s-]/g,'')))er.phone='Enteravalid 10-digitmobilenumber.';
if(!v.dept)er.dept='Selectyourdepartment.';
if(!v.year)er.year='Selectyouryear.';
['name','email','roll','phone','dept','year'].forEach(function(k){varel=$('#f-'+k);el.classList.toggle('bad',!!
er[k]);el.parentNode.querySelector('.err').textContent=er[k]||''});
varfirst=Object.keys(er)[0];if(first){$('#f-'+first).focus();return}
if(regs.some(function(r){return r.eventId===e.id&&r.email===v.email})){er.email='Thisemailisalready registered forthisevent.';$('#f-
email').classList.add('bad');$('#f-email').parentNode.querySelector('.err').textContent=er.email;return}
if(count(e.id)>=e.capacity){toast('Sorry, thiseventjustfilled up.');openEvent(e.id);drawGrid();return}
regs.push({id:'r'+Date.now(),eventId:e.id,name:v.name,email:v.email,roll:v.roll,phone:v.phone,dept:v.dept,year:v.year,at:new Date().toISOString()});
save();user=v.email;store(UK,user);$('#lookup').value=user;
dlg.close();toast('You’reregistered for'+e.title+'.');refresh();
}
