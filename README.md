# Cambridge One → PDF

Patch for the **Cambridge One Desktop App** that generates a real PDF next to every ebook you download for offline use.

ما يعمله التعديل: كل ما تنزّل كتاب للقراءة بدون إنترنت، بيتولّد ملفا PDF تلقائيًا في مجلد الكتاب:

| الملف | الوصف |
|---|---|
| `<code>.pdf` | سريع وخفيف (صور دقة متوسطة) |
| `<code>-hq.pdf` | جودة عالية (صور بدقة xlrg) |

المميزات:

- روابط الصفحات الداخلية شغّالة داخل الـ PDF (تنقل بين الصفحات)
- فهرس جانبي (bookmarks) مأخوذ من فهرس الكتاب الأصلي
- طبقة نصوص شفافة للبحث والنسخ — إذا كانت حزمة الكتاب تتضمن بيانات النصوص
- الكتب تتخزن في `%APPDATA%\CambridgeOne\books` بدل مجلد عشوائي على جذر القرص

## التثبيت / Install

1. نزّل **Cambridge One Desktop App** الرسمي وثبّته.
2. نزّل **Node.js** من https://nodejs.org
3. نزّل هالمشروع (Code → Download ZIP) وفك الضغط.
4. أغلق التطبيق إذا كان شغال، وشغّل `patch.bat` (دبل-كليك).

من السطر:

```bat
patch.bat
:: أو مع مسار التثبيت يدويًا:
node patch.js "D:\_PROJECTS\CambridgeOne"
```

## التراجع / Restore

شغّل `unpatch.bat` — بيرجع `app.asar` الأصلي من النسخة الاحتياطية `app.asar.bak`.

## English

- Patches `resources/app.asar` in place (surgical rewrite, original preserved as `app.asar.bak`).
- Injects a hook in the book download pipeline that runs `book-to-pdf.js` when a download completes.
- `book-to-pdf.js` embeds the book's page JPEGs into a PDF (DCTDecode, no re-encoding), adds internal page links from `book-overlays.json`, an outline from `toc_metadata.json`, and an invisible text layer from `pagetext-meta/` when present.
- Books/PDFs land in `%APPDATA%\CambridgeOne\books\<productCode>\`.
- Idempotent: running it again just updates the injected module.

Tested against CambridgeOne **2.54.0**. If the app updates and file layout changes, the patcher prints a clear "unsupported version" message instead of breaking anything — and `unpatch.bat` restores the original.

## ملاحظات / Notes

- للاستخدام الشخصي فقط. ما في توزيع لأي ملفات تملكها Cambridge — الأداة بتعدّل نسختك المثبتة محليًا.
- الكتب اللي حزمتها ما فيها `pagetext-meta` بتضل الـ PDFs تبعها صور فقط (الروابط والفهرس بيشتغلو عادي).
