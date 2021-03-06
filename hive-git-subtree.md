همانطور که [قبلا]  [هم] در هایو نوشته بودم [git] امکان به اسم [submodule] دارد که امکن ترکیب ماژول‌های نرم‌افزار را به ما می‌دهد. در همان حدود ما در تیم توسعه‌مان از [submodule] برای توسعه نسل بعدی نرم‌افزارمان استفاده کردیم. بعد از گذشت حدود ۱۰ ماه از استفاده از این ویژگی [git] در این نوشته، مشکلات استفاده از [submodule] بصورت خلاصه بیان شده و راه‌حل جایگزینی ارائه می‌گردد. مهم‌ترین مشکل استفاده از [submodule] ها پیچیدگی روند کار با آن است. تیم برای انجام کارهای معمول خود نیاز به دانش بیشتری دارد. در ابتدا افرادی که درگیر این پروژه جدید بودند به دو نفر از اعضای تیم محدود می‌شد که بخشی از کارشان توسعه این پروژه به عنوان تحقیق و توسعه بود. اما با بالغ شدن پروژه و توسعه آن توسط تمام اعضای تیم مشکلات این روند کاری پیچیده خود را نشان دادند. حال روندی که برای جایگزین کردن پیشنهاد میشود استفاده از [subtree] هاست. 

دستور [subtree] یک مخزن [git] را به عنوان یک زیرشاخه به مخزن اصلی اضافه میکند، همچنین کار بعدی آن این است که می‌توان زیر شاخه‌ای را از مخزن بزرگ جدا کرده تا تبدیل به یک مخزن مجزا شود. معمولا در مواردی که چند مخزن (یکی اصلی و مابقی به عنوان ماژول) وجود دارد از این روش استفاده می‌کنند. بدین صورت که یک مخزن اصلی(بزرگ) دارند که همه مخازن ماژول‌ها به عنوان [subtree] اضافه شده‌اند. روند کار یک برنامه نویس با مخزن اصلی بصورت عادی است و با همان روندهای آشنای [git]، توسعه را شروع و ادامه می‌دهد. حال در زمان‌هایی می‌توان تغییرات زیر شاخه‌ها را به مخازن مجزایشان توسط افرادی که تجربه‌بیشتری دارند و یا حتی با استفاده از چند script ساده ارسال نمود. 

با این توضیحات دیدن یک نمونه عملی هم خالی از لطف نیست. در این مثلا قرار است یک [مخزن اصلی] وجود داشته باشد که شامل [مخزن تمامی پست‌های وبلاگ] بنده باشد.



## اضافه کردن یک [subtree]

اضافه کردن یک [subtree] کار راحتی است. کافی است که به خط فرمان رفته و از دستور `git subtree add` استفاده کنید مثال این دستور به شکل زیر است:

```bash
$ git subtree add --prefix blog https://github.com/yazdan/blogPosts master
git fetch https://github.com/yazdan/blogPosts master
warning: no common commits
remote: Counting objects: 232, done.
remote: Total 232 (delta 0), reused 0 (delta 0), pack-reused 232
Receiving objects: 100% (232/232), 145.35 KiB | 36.00 KiB/s, done.
Resolving deltas: 100% (98/98), done.
From https://github.com/yazdan/blogPosts
 * branch            master     -> FETCH_HEAD
Added dir 'blog'
```
بعد از اجرای درست این دستور شاخه‌ای به نام blog به مخزن اصلی اضافه شده و تمامی سابقه مخزن ماژول نیز به عنوان سابقه مخزن اصلی اضافه می‌شود. در این مرحله نیاز نیست کار دیگری انجام دهید. تمامی ماژول‌ها را به همین منوال اضافه می‌شود.

## کار با مخزنی که [subtree] دارد

کار با مخازنی که [subtree] دارد عینا شبیه مخازن عادی است و هیچ تفاوتی با مخازن عادی ندارد

## بازگرداندن تغییرات به ماژول‌ها

برای بازگرداندن تغییرات به ماژول‌ها می‌توانید از دستور زیر استفاده کنید

```bash
$ git remote add blogposts-github https://github.com/yazdan/blogPosts
$ git subtree push --prefix blog blogposts-github master
git fetch https://github.com/yazdan/blogPosts master
git push using:  blogposts-github master
Everything up-to-date
```


##نکات قابل توجه هنگام استفاده از [subtree]ها
همانطور که در مورد تمام ابزارهای تکنولوژیک صدق می‌کنید هریک از این ابزارها علاوه بر حل کردن مشکلاتی، ممکن است پیچیدگی‌هایی را ایجاد کنند. قطعا استفاده از [subtree] ساده‌تر و آسان‌تر از استفاده از [submodule] هاست. ولی به خودی خود مشکلاتی نیز دارد

- کل سابقه مخزن ماژول به مخزن مادر اضافه می‌شود. که در صورت وجود سابقه‌های زیاد این عمل باعث سنگین شدن مخزن اصلی می‌شود.
- روند merge کردن و رفع conflict ها پیچیده‌تر حالت عادی است.

در مجموع استفاده از [subtree] به استفاده از [submodule] بخاطر سادگی بیشتر ارجحیت دارد.

## منابع و خواندن بیشتر
منابع این متن در زیر آمده است:

- این [پست وبلاگی] که به توضیح تجربه واقعی استفاده پرداخته.
- این [مقاله] در [مدیوم] هم قابل توجه است.

[git]:https://en.wikipedia.org/wiki/Git_(software)
[submodule]:https://git-scm.com/docs/git-submodule
[subtree]:https://github.com/git/git/blob/master/contrib/subtree/git-subtree.txt
[clone]:http://git-scm.com/docs/git-clone
[branch]:https://git-scm.com/docs/git-branch
[push]:https://git-scm.com/docs/git-push
[commit]:https://git-scm.com/docs/git-commit

[پست وبلاگی]:https://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/
[مقاله]:https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec#.so74vboll
[مدیوم]:https://medium.com/@v/git-subtrees-a-tutorial-6ff568381844#.ikqf1mh7h
[قبلا]:https://hive.ir/%d9%85%d8%b9%d8%b1%d9%81%db%8c-%d9%86%d8%ad%d9%88%d9%87-%d8%a7%d8%b3%d8%aa%d9%81%d8%a7%d8%af%d9%87-%d8%b2%db%8c%d8%b1%d9%85%d8%ae%d8%a7%d8%b2%d9%86-submodule-%d9%87%d8%a7-%d8%af/
[هم]:https://hive.ir/%d9%85%d8%b9%d8%b1%d9%81%db%8c-%d8%b1%d9%88%d9%86%d8%af%d9%87%d8%a7%db%8c-%da%a9%d8%a7%d8%b1-%d8%a8%d8%a7-git/
[مخزن اصلی]:https://github.com/yazdan/parent
[مخزن تمامی پست‌های وبلاگ]:https://github.com/yazdan/blogPosts

