## 💻 The Abstraction: The Process
### 📙 Operating Systems: Three Easy Pieces (OSTEP) 

در این بخش یکی از اساسی‌ ترین abstraction هایی را که OS به کاربران ارائه می‌ دهد بررسی می‌ کنیم یعنی**process**. تعریف process به‌ طور غیررسمی، کاملاً ساده است یک process همان running program یا برنامه در حال اجرا است. خود program چیزی است که فقط روی disk قرار دارد، مجموعه‌ ای از instruction ها و شاید مقداری static data است و منتظر است تا وارد عمل شود. این operating system است که این bytes را برمی‌ دارد و اجرا می‌ کند و program را به چیزی مفید تبدیل می‌ کند.

اغلب می‌خواهیم بیش از یک program را هم‌ زمان اجرا کنیم مثلاً desktop یا laptop خودتان را در نظر بگیرید که ممکن است بخواهید روی آن یک web browser ، یک mail program ، یک game ، یک music player و مانند آن را اجرا کنید. در واقع ، یک سیستم معمولی ممکن است به‌ظاهر ده‌ ها یا حتی صد ها process را هم‌ زمان اجرا کند. انجام این کار استفاده از سیستم را آسان می‌ کند ، چون دیگر لازم نیست نگران باشید که آیا یک CPU در دسترس است یا نه فقط program ها را اجرا می‌ کنید. ازاین‌ رو، چالش ما این است که با اینکه فقط چند physical CPU در دسترس است ، OS چگونه می‌ تواند توهم وجود تعداد تقریباً بی‌نهایتی از CPU ها را فراهم کند ؟

سیستم عامل این توهم را با **virtualizing the CPU** ایجاد می‌ کند. با اجرای یک process ، سپس متوقف کردن آن و اجرای process دیگر و ادامه دادن همین روند ، OS می‌ تواند این توهم را ایجاد کند که چندین virtual CPU وجود دارد ، در حالی که در واقع فقط یک physical CPU یا چند CPU وجود دارد. این تکنیک پایه ، که **time sharing** نامیده می‌ شود، به کاربران اجازه می‌ دهد هر تعداد process را که می‌ خواهند هم‌ زمان اجرا کنند و هزینه آن performance است ، چون اگر CPU ها به اشتراک گذاشته شوند ، هر process با سرعت کمتری اجرا خواهد شد.

برای پیاده‌ سازی virtualization مربوط به CPU و پیاده‌ سازی خوب آن ، OS هم به مقداری low-level machinery نیاز دارد و هم به مقداری high-level intelligence ، که low-level machinery را **mechanism** می‌ نامیم.  mechanism ها روش‌ ها یا protocol های سطح‌پایینی هستند که یک قطعه لازم از functionality را پیاده‌ سازی می‌ کنند. مثلاً در ادامه یاد می‌ گیریم چگونه یک **context switch** پیاده‌ سازی کنیم که به OS امکان می‌ دهد اجرای یک program را روی یک CPU مشخص متوقف کند و اجرای program دیگری را آغاز کند این mechanism مربوط به time-sharing توسط همه OS های مدرن به کار می‌ رود.

> 💡 TIP : USE TIME SHARING AND SPACE SHARING
>
> یکی از اساسی‌ ترین تکنیک‌ها **Time sharing** است که یک OS برای به‌ اشتراک‌ گذاری یک resource به کار می‌ برد. با اجازه دادن به اینکه resource برای مدت کوتاهی توسط یک entity استفاده شود ، سپس برای مدت کوتاهی توسط entity دیگری استفاده شود و همین روند ادامه پیدا کند ، resource موردنظر مثلاً CPU یا یک network link می‌ تواند میان افراد بسیاری به اشتراک گذاشته شود. همتای طبیعی time sharing هم space sharing است که در آن یک resource به‌ صورت مکانی در space میان کسانی که می‌ خواهند از آن استفاده کنند تقسیم می‌ شود. مثلاً disk space به‌ طور طبیعی یک resource از نوع space-shared است ، چون وقتی یک block به یک file اختصاص داده شد ، بعید است تا زمانی که کاربر آن را حذف نکند به file دیگری اختصاص داده شود.

بالای این mechanism ها ، مقداری intelligence در OS قرار دارد که به شکل **policy** ها ظاهر می‌ شود. policy ها الگوریتم‌ هایی برای تصمیم‌ گیری درباره یک موضوع خاص درون OS هستند. مثلاً با فرض وجود تعدادی program که می‌ توانند روی یک CPU اجرا شوند ، OS کدام program را باید اجرا کند؟ یک **scheduling policy** در OS این تصمیم را می‌ گیرد و احتمالاً برای تصمیم‌ گیری از اطلاعات تاریخی مثلاً کدام program در یک دقیقه گذشته بیشتر اجرا شده است؟ ، دانش درباره workload مثلاً چه نوع program هایی اجرا می‌شوند و performance metric ها مثلاً سیستم برای interactive performance بهینه می‌ شود یا برای throughput ؟ استفاده می‌ کند.

---
### 🔸 4.1 The Abstraction: A Process

به abstraction که OS از یک program در حال اجرا ارائه می‌ دهد چیزی است که آن را **process** می‌ نامیم. همان‌ طور که بالا گفتیم ، process صرفاً یک program در حال اجراست. در هر لحظه از زمان ، می‌ توانیم با فهرست کردن بخش‌ های مختلف سیستم که process در طول اجرای خود به آن‌ ها دسترسی دارد یا بر آن‌ ها اثر می‌ گذارد ، یک process را خلاصه کنیم. برای درک اینکه چه چیزی یک process را تشکیل می‌ دهد ، باید **machine state** آن را بفهمیم یعنی آنچه یک program هنگام اجرا می‌ تواند بخواند یا به‌ روزرسانی کند. در هر لحظه معین، کدام بخش‌ های ماشین برای اجرای این program مهم هستند؟

یک جزاز machine state که یک process را تشکیل می‌ دهد **memory** آن است. instruction ها در memory قرار دارند و داده‌ هایی که program در حال اجرا می‌ خواند و می‌ نویسد نیز در memory قرار دارند. بنابراین memory که process می‌ تواند به آن address بزند **address space** آن نامیده می‌شود و بخشی از process است. همچنین **register** ها بخشی از machine state مربوط به process هستند چون بسیاری از instruction ها به‌ صراحت register ها را می‌ خوانند یا به‌ روزرسانی می‌ کنند و بنابراین واضح است که register ها برای اجرای process مهم‌ اند.

توجه کنید که برخی register های به‌ خصوص ، بخشی از این machine state را تشکیل می‌ دهند. مثلاً program counter (PC) که گاهی instruction pointer یا IP نیز نامیده می‌ شود به ما می‌ گوید کدام instruction از program در حال حاضر در حال اجراست.  به‌ طور مشابه ، یک stack pointer و frame pointer مرتبط با آن برای مدیریت stack مربوط به function parameter ها ، local variable ها و return address ها به کار می‌ روند.

> 💡 TIP : SEPARATE POLICY AND MECHANISM
>
> در بسیاری از operating system ها ، یک الگوی طراحی رایج ، جدا کردن high-level policy ها از low-level mechanism های آن‌ هاست. می‌ توانید mechanism را پاسخ‌ دهنده یک پرسش «چگونه» درباره یک سیستم در نظر بگیرید مثلاً یک operating system چگونه یک context switch انجام می‌ دهد ؟ ، policy پاسخ پرسش «کدام» را می‌ دهد مثلاً operating system همین حالا کدام process را باید اجرا کند ؟ جدا کردن این دو، تغییر policy ها را بدون نیاز به بازاندیشی در mechanism آسان می‌ کند و بنابراین نوعی **modularity**است که یک اصل کلی در طراحی software به شمار می‌ رود.

در نهایت ، program ها اغلب به persistent storage device ها نیز دسترسی دارند. چنین اطلاعات I/O ممکن است شامل فهرستی از file هایی باشد که process در حال حاضر باز کرده است.

---
### 🔸 4.2 Process API

اگرچه بحث درباره یک process API واقعی را به فصلی بعدی موکول می‌ کنیم ، در اینجا ابتدا تصوری از آنچه باید در هر interface از یک operating system وجود داشته باشد ارائه می‌ دهیم. این API ها به شکلی در هر operating system مدرنی در دسترس هستند :

#### 🔻 Create
یک operating system باید شامل روشی برای ایجاد process های جدید باشد. وقتی دستوری را در shell تایپ می‌ کنید یا روی آیکون یک application دوبار کلیک می‌ کنید ، OS فراخوانی می‌ شود تا یک process جدید برای اجرای program که مشخص کرده‌ اید ایجاد کند.

#### 🔻 Destroy
همان‌ طور که یک interface برای ایجاد process وجود دارد ، سیستم‌ ها یک interface برای نابود کردن اجباری process ها نیز فراهم می‌ کنند. البته بسیاری از process ها اجرا می‌ شوند و وقتی کارشان تمام شد خودشان exit می‌کنند اما وقتی چنین نمی‌ کنند ، کاربر ممکن است بخواهد آن‌ ها را kill کند بنابراین داشتن یک interface برای متوقف کردن یک runaway process بسیار مفید است.

#### 🔻 Wait
گاهی مفید است که منتظر بمانیم تا یک process اجرای خود را متوقف کند بنابراین اغلب نوعی interface برای waiting ارائه می‌ شود.

#### 🔻 Miscellaneous Control
جدا از kill کردن یا wait کردن برای یک process ، گاهی کنترل‌ های دیگری هم ممکن است. مثلاً بیشتر operating system ها نوعی روش برای suspend کردن یک process که متوقف کردن اجرای آن برای مدتی و سپس resume کردن و ادامه اجرای آن process را فراهم می‌ کنند.

#### 🔻 Status
معمولاً interface هایی هم برای دریافت اطلاعات status درباره یک process وجود دارد ، مثل اینکه چه مدت اجرا شده است یا در چه state قرار دارد.

<img width="100%" height="632" alt="image" src="https://github.com/user-attachments/assets/4d88c68f-3670-4663-8820-136d4c5fad01" />

در این شکل ، program روی Disk قرار دارد و شامل code و static data است. فرایند Loading ، که program روی disk را برمی‌ دارد و آن را در address space مربوط به process را می‌ خواند در نتیجه ، در Memory هم process شامل code ، static data ، heap و stack است و CPU آن را اجرا می‌ کند.

---
### 🔸 4.3 Process Creation: A Little More Detail

یکی از رازهایی که باید بپردازیم این است که program ها چگونه به process ها تبدیل می‌ شوند. به‌ طور مشخص، OS چگونه یک program را راه‌ اندازی و اجرا می‌ کند ؟ ایجاد process در واقع چگونه کار می‌ کند ؟

اولین کاری که OS برای اجرای یک program باید انجام دهد این است که code آن و هر static data مثلاً متغیرهای مقداردهی‌ شده initialized variables را به memory ، یعنی به درون address space مربوط به process را load کند. program ها در ابتدا روی disk یا در برخی سیستم‌ های مدرن ، روی SSD های مبتنی بر flash به شکلی از executable format قرار دارند بنابراین فرایند load کردن یک program و static data به memory وابسته به این است که OS آن bytes را از disk بخواند و آن‌ ها را در جایی از memory قرار دهد همان‌ طور که در شکل ۴٫۱ نشان داده شده است.

در operating system های اولیه یا ساده ، فرایند loading به‌ صورت eagerly انجام می‌ شد ، یعنی همه‌ چیز یکجا و پیش از اجرای program که OS های مدرن این فرایند را به‌ صورت lazily انجام می‌ دهند ، یعنی قطعه‌ هایی از code یا data را فقط هنگامی که در طول اجرای program به آن‌ ها نیاز است load می‌ کنند. برای درک واقعی نحوه کار lazy loading قطعه‌ های code و data ، باید درباره machinery مربوط به paging و swapping بیشتر بدانید. موضوعاتی که در آینده ، هنگام بحث درباره virtualization مربوط به memory ، پوشش خواهیم داد. فعلاً فقط به یاد داشته باشید که پیش از اجرای هر چیز، OS بدیهی است که باید کاری انجام دهد تا bit های مهم program را از disk به memory بیاورد.

وقتی code و static data در memory load شدند ، چند کار دیگر هست که OS باید پیش از اجرای process انجام دهد. مقداری memory باید برای run-time stack یا فقط stack برنامه تخصیص داده شود. همان‌ طور که احتمالاً از قبل می‌ دانید ، program های C از stack برای local variable ها ، function parameter ها و return address ها استفاده می‌ کنند ، OS این memory را تخصیص می‌ دهد و به process می‌ دهد. OS احتمالاً stack را با argument ها نیز initialize می‌ کند به‌ طور مشخص، parameter های تابع()main یعنی argc و آرایه argv را پر می‌ کند.

همچنین OS چند کار initialization دیگر انجام می‌ دهد ، به‌ ویژه آنچه به input/output (I/O) مربوط می‌ شود. مثلاً در سیستم‌ های UNIX ، هر process به‌ طور پیش‌ فرض سه file descriptor باز دارد ، برای standard input ، standard output و standard error که این descriptor ها به program ها اجازه می‌ دهند به‌ راحتی ورودی را از terminal بخوانند و خروجی را روی صفحه چاپ کنند. درباره I/O ، file descriptor ها و مواردی از این دست در بخش سوم کتاب درباره persistence بیشتر خواهیم آموخت.

با load کردن code و static data به memory ، با ایجاد و initialize کردن یک stack و با انجام کارهای دیگر مرتبط با راه‌ اندازی I/O هم ، OS اکنون زمینه را برای اجرای program آماده کرده است. بنابراین یک وظیفه آخر دارد ، شروع اجرای program از entry point یعنی()main. با پریدن به روتین()main از طریق یک mechanism تخصصی که در فصل بعد درباره آن بحث می‌کنیم ، OS کنترل CPU را به process تازه‌ ساخته‌ شده منتقل می‌ کند و بدین ترتیب program اجرای خود را آغاز می‌ کند.

---
### 🔸 4.4 Process States

اکنون که تصوری از اینکه process چیست و چگونه ایجاد می‌ شود داریم ، بیایید درباره state های مختلفی که یک process می‌ تواند در یک زمان مشخص در آن‌ها باشد صحبت کنیم. این مفهوم که یک process می‌ تواند در یکی از این state ها باشد در سیستم‌ های computer اولیه پدید آمد. در یک دید ساده‌ شده ، یک process می‌ تواند در یکی از سه state باشد :

#### 🔻 Running State
در state مربوط به running ، یک process روی یک processor در حال اجراست. این یعنی instruction ها را execute می‌ کند.

#### 🔻 Ready State
در state مربوط به ready ، یک process آماده اجراست ، اما به دلیلی OS ترجیح داده است در همین لحظه آن را اجرا نکند.

#### 🔻 Blocked State
در state مربوط به blocked ، یک process عملیاتی از نوعی انجام داده است که باعث می‌ شود تا زمان وقوع رویداد دیگری آماده اجرا نباشد. مثلاً وقتی یک process یک I/O request به disk آغاز می‌ کند ، blocked می‌ شود و در نتیجه process دیگری می‌ تواند از processor استفاده کند.

<img width="100%" height="355" alt="image" src="https://github.com/user-attachments/assets/970f1763-3d89-4c0e-8e84-6c0988761dd8" />

اگر بخواهیم این state ها را روی یک گراف رسم کنیم ، به نمودار شکل ۴٫۲ می‌رسیم. همان‌ طور که در نمودار می‌ بینید، یک process می‌ تواند به صلاح‌ دید OS بین state های ready و running جابه‌جا شود. جابه‌جا شدن از ready به running یعنی** process scheduled** شده است. جابه‌جا شدن از running به ready یعنی **process descheduled** شده است. وقتی یک process blocked می‌ شود مثلاً با آغاز یک عملیات I/O که OS آن را تا زمان وقوع رویدادی مثلاً تکمیل I/O در همان حالت نگه می‌ دارد. در آن نقطه ، process دوباره به state مربوط به ready می‌ رود و بلافاصله دوباره به running می رود ، اگر OS چنین تصمیمی بگیرد.

---
### 🔸 4.5 Data Structures

خود OS یک program است و مثل هر program ، مجموعه‌ ای از data structure های کلیدی دارد که اطلاعات مرتبط مختلف را پیگیری می‌ کنند. مثلاً برای پیگیری state هر process که OS احتمالاً نوعی **process list** برای همه process هایی که ready هستند نگه می‌ دارد ، به‌علاوه مقداری اطلاعات اضافی برای پیگیری اینکه کدام process در حال حاضر در حال اجراست. OS باید به نحوی process های blocked را هم پیگیری کند. وقتی یک I/O event تکمیل می‌ شود ، OS باید مطمئن شود process درست را برای اجرای مجدد آماده می‌ کند.  شکل ۴٫۳ نشان می‌ دهد که یک OS در xv6 kernel چه نوع اطلاعاتی را درباره هر process باید پیگیری کند. ساختارهای process مشابهی در operating system های واقعی مثل Linux ، Mac OS X یا Windows وجود دارد ، آن‌ ها را جست‌ وجو کنید و ببینید چقدر پیچیده‌ ترند.

```c
// the registers xv6 will save and restore
// to stop and subsequently restart a process
struct context {
   int eip;
   int esp;
   int ebx;
   int ecx;
   int edx;
   int esi;
   int edi;
   int ebp;
};

// the different states a process can be in
enum proc_state { UNUSED, EMBRYO, SLEEPING,
                  RUNNABLE, RUNNING, ZOMBIE };
// the information xv6 tracks about each process
// including its register context and state
struct proc {
   char *mem;                  // Start of process memory
   uint sz;                    // Size of process memory
   char *kstack;               // Bottom of kernel stack
                               // for this process
   enum proc_state state;      // Process state
   int pid;                    // Process ID
   struct proc *parent;        // Parent process
   void *chan;                 // If non-zero, sleeping on chan
   int killed;                 // If non-zero, have been killed
   struct file *ofile[NOFILE]; // Open files
   struct inode *cwd;          // Current directory
   struct context context;     // Switch here to run process
   struct trapframe *tf;       // Trap frame for the
                               // current interrupt
};

Figure 4.3: The xv6 Proc Structure
```

از روی شکل می‌ توانید چند قطعه اطلاعات مهم را ببینید که OS درباره یک process پیگیری می‌ کند. **register context** برای یک process متوقف‌ شده ، محتویات register state آن را نگه می‌ دارد. وقتی یک process متوقف می‌ شود ، register state آن در این مکان از memory ذخیره می‌ شود. OS با restore این register ها یعنی قرار دادن مقادیر آن‌ ها در register های فیزیکی واقعی می‌ تواند اجرای process را از سر بگیرد. در فصل‌های آینده درباره این تکنیک ، که **context switch** نام دارد ، بیشتر خواهیم آموخت.

همچنین از شکل می‌ توانید ببینید که state های دیگری هم وجود دارند که یک process می‌ تواند در آن‌ ها باشد ، فراتر از running ، ready و blocked. گاهی یک سیستم یک state اولیه دارد که process هنگام ایجاد شدن در آن قرار می‌ گیرد. همچنین یک process ممکن است در یک state نهایی قرار داده شود که در آن exit کرده است اما هنوز clean up نشده است که در سیستم‌های مبتنی بر UNIX این state را **zombie state** می‌نامند. این state نهایی می‌ تواند مفید باشد ، چون به process های دیگر معمولاً parent که process را ایجاد کرده است اجازه می‌ دهد return code مربوط به process را بررسی کنند و ببینند آیا process تازه‌ تمام‌ شده با موفقیت اجرا شده است یا نه ، معمولاً program ها در سیستم‌ های مبتنی بر UNIX وقتی کاری را با موفقیت انجام داده‌ اند صفر بر می‌گردانند و در غیر این صورت مقداری غیرصفر. وقتی کار تمام شد ، parent یک فراخوانی نهایی مثلاً()wait انجام می‌ دهد تا منتظر تکمیل child بماند و همچنین به OS نشان دهد که می‌ تواند هر data structure مرتبطی را که به process اکنون‌ منقضی‌ شده اشاره می‌ کرد پاک‌ سازی کند.

> ASIDE : DATA STRUCTURE — THE PROCESS LIST
> سیستم عامل ها شامل data structure های مهم گوناگونی هستند که در این بخش ها درباره آن‌ها بحث خواهیم کرد. **process list** چنین ساختاری است. این یکی از ساده‌ ترین آن‌ها ست ، اما قطعاً هر OS که توانایی اجرای چند program به‌ صورت هم‌ زمان را دارد برای پیگیری همه program های در حال اجرای سیستم ، چیزی شبیه این ساختار خواهد داشت. گاهی مردم به ساختار منفردی که اطلاعات مربوط به یک process را ذخیره می‌ کند **Process Control Block (PCB)** می‌ گویند ، که روشی شیک برای یک C structure است که اطلاعات مربوط به هر process را در خود دارد.

--- 
### 🔸 4.6 Summary

ما اساسی‌ ترین abstraction مربوط به OS را معرفی کردیم به نام **process**. این مفهوم به‌ سادگی به‌ عنوان یک program در حال اجرا دیده می‌ شود. با در نظر داشتن این دید مفهومی ، اکنون به جزئیات اصلی می‌ رویم یعنی low-level mechanism های لازم برای پیاده‌ سازی process ها و high-level policy های لازم برای زمان‌ بندی schedule هوشمندانه آن‌ ها. با ترکیب mechanism ها و policy ها ، درک خود را از اینکه یک operating system چگونه CPU را virtualize می‌ کند خواهیم ساخت.
