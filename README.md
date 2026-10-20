# QA Agent — AI-Powered Test Failure Analysis

**An AI-assisted QA tool that analyzes Jenkins test results, validates report consistency, and helps investigate test failures.**

## English

### Overview

QA Agent is a Python-based project designed to assist with the analysis of automated UI test results generated through Jenkins, Maven, TestNG, and Selenium WebDriver.

The project addresses an important challenge in test reporting: different report files and console summaries may contain inconsistent results. Instead of immediately sending the available data to an AI model, the agent performs validation checks first.

The goal is to support evidence-based failure investigation, distinguish confirmed findings from assumptions, and help QA engineers decide what to investigate next.

### Workflow

1. **Test Execution:** Selenium UI tests run through Jenkins using Maven and TestNG.
2. **Report Generation:** Maven Surefire generates XML test reports.
3. **XML Analysis:** Python reads the test report and extracts test counts and failed test details.
4. **Console Log Analysis:** The agent parses Jenkins console output, identifies build status, and extracts test summaries and error details.
5. **Data Consistency Validation:** XML results are compared with the available console summaries.
6. **AI-Assisted Analysis:** If the validation rules allow the analysis to proceed, the collected evidence is sent to the OpenAI API.
7. **Output:** The analysis is displayed in the console, highlighting failures, discrepancies, possible explanations, and recommended investigation steps.

### Current Features

- Parses Maven Surefire XML test reports.
- Extracts failed test information.
- Reads and analyzes Jenkins console logs.
- Compares test, failure, error, and skipped counts.
- Detects the Jenkins build result.
- Identifies discrepancies between XML reports and console summaries.
- Uses a validation gate before AI analysis.
- Uses OpenAI to support failure investigation.
- Encourages separation of confirmed evidence from assumptions.
- Generates analysis in Turkish.

### Example Investigation

The agent has been used to investigate issues such as:

- A timeout while waiting for a Login link to become clickable.
- A search test expecting the `Search - iPhone` page title but receiving `My Account` instead.
- Different test counts appearing in XML reports and Jenkins console summaries.

These observations help guide investigation, but they do not automatically establish the root cause. Further evidence is required before confirming why a failure occurred.

### Technology Stack

- Python
- OpenAI API
- Selenium WebDriver
- TestNG
- Maven
- Jenkins
- Maven Surefire XML reports

### Engineering Principles

- **Validate before analyzing:** Check report consistency before sending data to the AI model.
- **Evidence over assumptions:** Separate observed facts from possible explanations.
- **No unsupported conclusions:** Do not present a suspected root cause as confirmed without evidence.
- **Preserve uncertainty:** Do not arbitrarily decide which source is correct when results conflict.
- **Human judgment matters:** Use AI as an investigation assistant, not as a replacement for QA engineering judgment.

### Current Status

The project currently supports XML and console-log parsing, an initial consistency validation process, and AI-assisted failure analysis.

Validation is still an evolving part of the project. Matching test counts alone does not prove that two files belong to the same Jenkins build. Stronger build identity and report provenance checks remain important areas for improvement.

### Planned Improvements

- Stronger validation of report and build identity.
- Structured analysis reports, including Word output.
- A dashboard for test results and failure trends.
- Automated Jenkins build triggering.
- Failure categorization and more targeted troubleshooting suggestions.
- Carefully controlled, evidence-based suggestions for potential code fixes.

### Project Goal

The goal is to develop a practical QA assistant that combines automated test reporting and AI-assisted investigation while keeping data consistency, evidence, uncertainty, and human judgment at the center of the process.


## Türkçe

### Proje Hakkında

QA Agent; Jenkins, Maven, TestNG ve Selenium WebDriver kullanılarak çalıştırılan otomasyon testlerinin sonuçlarını analiz etmeye yardımcı olmak amacıyla geliştirdiğim Python tabanlı bir projedir.

Projenin çıkış noktası, test raporları ve konsol kayıtları arasında oluşabilecek tutarsızlıklardır. Farklı kaynaklardan gelen sonuçları doğrudan bir yapay zekâ modeline göndermek yerine, önce verilerin tutarlılığını kontrol etmeyi amaçlar.

Temel hedefim; test hatalarını kanıta dayalı şekilde incelemeyi kolaylaştırmak, doğrulanmış bulgularla varsayımları birbirinden ayırmak ve QA mühendisinin sonraki inceleme adımlarını belirlemesine yardımcı olmaktır.

### Çalışma Akışı

1. **Testlerin Çalıştırılması:** Selenium UI testleri, Jenkins üzerinden Maven ve TestNG kullanılarak çalıştırılır.
2. **Raporların Oluşturulması:** Maven Surefire, test sonuçlarını XML raporlarına kaydeder.
3. **XML Analizi:** Python, test raporunu okuyarak toplam test sayılarını ve başarısız testlerin ayrıntılarını çıkarır.
4. **Konsol Kayıtlarının Analizi:** Agent, Jenkins konsol çıktısını inceler; build durumunu, test özetlerini ve hata ayrıntılarını belirler.
5. **Veri Tutarlılığı Kontrolü:** XML raporundaki sonuçlar, konsoldaki test özetleriyle karşılaştırılır.
6. **Yapay Zekâ Destekli Analiz:** Doğrulama kuralları analize izin verirse elde edilen bulgular OpenAI API'ye gönderilir.
7. **Sonuçların Sunulması:** Analiz konsolda gösterilir; başarısız testler, veri tutarsızlıkları, olası açıklamalar ve izlenebilecek inceleme adımları belirtilir.

### Mevcut Özellikler

- Maven Surefire XML test raporlarını okuma.
- Başarısız test bilgilerini çıkarma.
- Jenkins konsol kayıtlarını inceleme.
- Toplam test, başarısız, hatalı ve atlanan test sayılarını karşılaştırma.
- Jenkins build sonucunu belirleme.
- XML raporu ile konsol özetleri arasındaki tutarsızlıkları tespit etme.
- Yapay zekâ analizinden önce doğrulama kontrolü uygulama.
- OpenAI API ile hata incelemesini destekleme.
- Kanıtlarla varsayımları birbirinden ayırmaya yönelik analiz istemleri kullanma.
- Analiz çıktısını Türkçe üretme.

### Örnek İncelemeler

Agent ile aşağıdaki gibi sorunlar incelenmiştir:

- Login bağlantısının tıklanabilir olmasını beklerken zaman aşımı oluşması.
- Arama testinde beklenen `Search - iPhone` sayfa başlığı yerine `My Account` başlığının alınması.
- XML raporları ile Jenkins konsol özetlerinde farklı test sayılarının görülmesi.

Bu bulgular, incelemenin hangi yönde ilerleyebileceğini gösterir; ancak tek başlarına hatanın kesin nedenini kanıtlamaz. Kök nedeni doğrulamak için ek kanıt gerekir.

### Kullanılan Teknolojiler

- Python
- OpenAI API
- Selenium WebDriver
- TestNG
- Maven
- Jenkins
- Maven Surefire XML raporları

### Mühendislik Yaklaşımı

- **Analizden önce doğrulama:** Verileri yapay zekâya göndermeden önce rapor tutarlılığını kontrol et.
- **Varsayım yerine kanıt:** Gözlemlenen gerçeklerle olası açıklamaları ayır.
- **Kanıtsız sonuç üretme:** Şüphelenilen bir nedeni doğrulanmış kök neden olarak sunma.
- **Belirsizliği koru:** Kaynaklar çeliştiğinde hangi kaynağın doğru olduğuna kanıtsız karar verme.
- **İnsan değerlendirmesini koru:** Yapay zekâyı QA mühendisinin yerine geçen bir sistem olarak değil, incelemeyi destekleyen bir yardımcı olarak kullan.

### Projenin Mevcut Durumu

Proje şu anda XML raporlarını ve konsol kayıtlarını okuyabilmekte, başlangıç seviyesinde tutarlılık kontrolleri yapmakta ve yapay zekâ destekli hata analizi sunmaktadır.

Veri doğrulama mekanizması geliştirilmeye devam etmektedir. Test sayılarının eşleşmesi, iki dosyanın aynı Jenkins build'ine ait olduğunu tek başına kanıtlamaz. Build kimliğinin ve raporların kaynağının daha güçlü biçimde doğrulanması, projenin geliştirilmesi gereken önemli alanlarındandır.

### Planlanan Geliştirmeler

- Rapor ve build kimliğini daha güçlü doğrulama.
- Word formatında yapılandırılmış analiz raporları.
- Test sonuçlarını ve hata eğilimlerini gösteren bir dashboard.
- Jenkins build'lerini otomatik tetikleme.
- Hataları kategorilere ayırma ve daha hedefli inceleme önerileri sunma.
- Kanıta dayalı ve kontrollü olası kod düzeltme önerileri geliştirme.

### Projenin Amacı

Amacım; otomasyon test raporlamasını yapay zekâ destekli hata incelemesiyle birleştiren, ancak veri tutarlılığını, kanıtları, belirsizliği ve insan değerlendirmesini sürecin merkezinde tutan kullanışlı bir QA yardımcısı geliştirmektir.

