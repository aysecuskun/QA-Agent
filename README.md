# QA Agent — AI-Powered Test Failure Analysis

**An AI-assisted QA tool that automates Jenkins test data collection, validates report consistency, and supports evidence-based test failure investigation.**

## English

### Overview

QA Agent is a Python-based project designed to assist with the investigation of automated UI test results generated through Jenkins, Maven, TestNG, and Selenium WebDriver.

The project addresses an important challenge in test reporting: different report files and console summaries may contain inconsistent results. Instead of immediately sending the available data to an AI model, the agent collects the relevant build artifacts, validates the available evidence, and only proceeds to AI analysis when the validation rules allow it.

The goal is to support evidence-based failure investigation, distinguish confirmed findings from assumptions, and help QA engineers decide what to investigate next.

### Workflow

The workflow is being developed incrementally, combining automated data collection, validation, and AI-assisted investigation.

1. **Jenkins Connection:** The Python application connects to Jenkins through its API using environment-based credentials.
2. **Build Discovery:** The agent identifies the latest completed build and retrieves its number, result, URL, and metadata.
3. **Automatic Data Collection:** The agent downloads the selected build's console log and archived Maven Surefire XML report.
4. **XML Parsing:** Python extracts test counts and available failed-test details from the XML report.
5. **Console Log Analysis:** The agent extracts test summaries and relevant error information from the Jenkins console output.
6. **Consistency Validation:** The agent compares test, failure, error, and skipped counts across the available report data.
7. **Conditional AI Analysis:** If the validation gate allows the workflow to proceed, the collected evidence is sent to the OpenAI API. If inconsistencies remain unresolved, the AI request is blocked.
8. **Investigation Output:** The agent presents findings and, when AI analysis is permitted, helps identify possible explanations and next investigation steps.

### Current Features

- Connects to Jenkins through its API.
- Retrieves the latest completed build information automatically.
- Downloads the selected build's console log.
- Downloads the archived Maven Surefire XML report.
- Parses XML test reports using Python's `xml.etree.ElementTree`.
- Extracts test counts and available failed-test information.
- Identifies the Jenkins build result.
- Extracts and compares test summaries from console logs.
- Detects discrepancies between XML reports and console summaries.
- Applies a validation gate before AI analysis.
- Blocks the OpenAI request when the current validation rules detect unresolved inconsistencies.
- Uses OpenAI to support evidence-based failure investigation.
- Produces analysis in Turkish.

### Example Investigation — Jenkins Build #16

The agent automatically collected the console log and Surefire XML report for Jenkins Build #16.

The XML report and console output contained the following results:

| Data source | Tests | Failures | Errors | Skipped |
| Surefire XML | 40 | 2 | 0 | 4 |
| Console summary 1 | 40 | 2 | 0 | 4 |
| Console summary 2 | 39 | 2 | 0 | 3 |

Although the first console summary matches the XML report, the second summary differs.

**Agent behavior:**
- Automatically collected the build data.
- Detected multiple console test summaries.
- Identified the discrepancy between the available results.
- Stopped the workflow before calling the OpenAI API.

This demonstrates an important design decision: the agent does not arbitrarily select a matching source and ignore conflicting evidence.

The discrepancy has not yet been conclusively explained. Further investigation is required to determine why the summaries differ and how the validation rules should handle this situation.

### Example Failure Scenarios

Previous investigations have included:

- A timeout while waiting for a Login link to become clickable.
- A search test expecting the `Search - iPhone` page title but receiving `My Account` instead.
- Different test counts appearing in XML reports and Jenkins console summaries.

These observations help guide investigation, but they do not automatically establish the root cause. Further evidence is required before confirming why a failure occurred.

### Technology Stack

- Python
- OpenAI API
- Jenkins REST API
- Selenium WebDriver
- TestNG
- Maven
- Maven Surefire XML reports
- XML parsing with `xml.etree.ElementTree`

### Engineering Principles

- **Validate before analyzing:** Check the available evidence before sending data to the AI model.
- **Evidence over assumptions:** Separate observed facts from possible explanations.
- **Fail safely:** Stop the workflow when the available data cannot be validated.
- **No arbitrary source selection:** Do not assume that a report is correct simply because it matches another source.
- **Preserve uncertainty:** A discrepancy is a finding to investigate, not proof of a specific root cause.
- **Human judgment matters:** Use AI as an investigation assistant, not as a replacement for QA engineering judgment.
- **Incremental development:** Implement, test, and verify each part of the workflow before expanding it.

### Current Status

The project currently supports automated Jenkins build discovery, console-log and Surefire XML collection, test report parsing, initial consistency validation, and conditional AI-assisted analysis.

The validation mechanism is still evolving. In Build #16, the agent detected two different console test summaries and correctly blocked the OpenAI request under the current validation rules.

Matching test counts alone does not prove that two files belong to the same build or represent the same execution scope. Understanding the report-generation process and strengthening build identity and report provenance checks remain important areas for improvement.

### Planned Improvements

- Investigate why multiple test summaries appear in the same Jenkins console log.
- Refine validation rules based on evidence from the build lifecycle and report-generation process.
- Strengthen build identity and report provenance validation.
- Generate structured investigation reports, including Word output.
- Develop a dashboard for test results and failure trends.
- Explore automated Jenkins build triggering.
- Improve failure categorization and troubleshooting suggestions.
- Explore carefully controlled, evidence-based suggestions for potential code fixes.

### Project Goal

The goal is to develop a practical QA assistant that combines automated test data collection, report validation, and AI-assisted investigation while keeping evidence, uncertainty, data consistency, and human judgment at the center of the process.

---

## Türkçe

### Proje Hakkında

QA Agent; Jenkins, Maven, TestNG ve Selenium WebDriver kullanılarak çalıştırılan otomasyon testlerinin sonuçlarını incelemeye yardımcı olmak amacıyla geliştirdiğim Python tabanlı bir projedir.

Projenin çıkış noktası, test raporları ve konsol kayıtları arasında oluşabilecek tutarsızlıklardır. Agent, mevcut verileri doğrudan yapay zekâya göndermek yerine ilgili build'e ait dosyaları otomatik olarak toplar, elde edilen kanıtları doğrular ve yalnızca doğrulama kuralları izin verdiğinde yapay zekâ analizine geçer.

Temel hedefim; test hatalarını kanıta dayalı şekilde incelemeyi kolaylaştırmak, doğrulanmış bulgularla varsayımları birbirinden ayırmak ve QA mühendisinin sonraki inceleme adımlarını belirlemesine yardımcı olmaktır.

### Çalışma Akışı

İş akışı; otomatik veri toplama, doğrulama ve yapay zekâ destekli inceleme adımlarını birleştirecek şekilde aşamalı olarak geliştirilmektedir.

1. **Jenkins Bağlantısı:** Python uygulaması, ortam değişkenleri üzerinden yapılandırılan kimlik bilgileriyle Jenkins API'sine bağlanır.
2. **Build'in Belirlenmesi:** Agent, son tamamlanan build'i bulur; numarasını, sonucunu, URL'sini ve diğer bilgilerini alır.
3. **Otomatik Veri Toplama:** İlgili build'in konsol logunu ve arşivlenmiş Maven Surefire XML raporunu indirir.
4. **XML Analizi:** Python, XML raporundan test sayılarını ve mevcut başarısız test ayrıntılarını çıkarır.
5. **Konsol Logunun İncelenmesi:** Agent, Jenkins konsol çıktısındaki test özetlerini ve ilgili hata kayıtlarını ayrıştırır.
6. **Tutarlılık Kontrolü:** Mevcut raporlardaki toplam test, başarısız, hatalı ve atlanan test sayıları karşılaştırılır.
7. **Koşullu Yapay Zekâ Analizi:** Doğrulama mekanizması izin verirse elde edilen kanıtlar OpenAI API'ye gönderilir. Çözümlenmemiş tutarsızlıklar varsa AI isteği engellenir.
8. **İnceleme Çıktısı:** Agent, bulguları sunar ve yapay zekâ analizine izin verildiğinde olası açıklamaları ve sonraki inceleme adımlarını belirlemeye yardımcı olur.

### Mevcut Özellikler

- Jenkins API'sine bağlanma.
- Son tamamlanan build bilgilerini otomatik olarak alma.
- İlgili build'in konsol logunu indirme.
- Arşivlenmiş Maven Surefire XML raporunu indirme.
- Python'ın `xml.etree.ElementTree` modülüyle XML raporlarını ayrıştırma.
- Test sayılarını ve mevcut başarısız test bilgilerini çıkarma.
- Jenkins build sonucunu belirleme.
- Konsol logundaki test özetlerini çıkarma ve karşılaştırma.
- XML raporları ile konsol özetleri arasındaki tutarsızlıkları tespit etme.
- Yapay zekâ analizinden önce doğrulama kontrolü uygulama.
- Mevcut doğrulama kuralları çözümlenmemiş tutarsızlıklar tespit ettiğinde OpenAI isteğini engelleme.
- Kanıta dayalı hata incelemesini OpenAI ile destekleme.
- Analiz çıktısını Türkçe üretme.

### Örnek İnceleme — Jenkins Build #16

Agent, Jenkins Build #16 için konsol logunu ve Surefire XML raporunu otomatik olarak topladı.

XML raporu ile konsol çıktısında aşağıdaki sonuçlar bulundu:

| Veri kaynağı | Test | Başarısız | Hata | Atlanan |
|---|---:|---:|---:|---:|
| Surefire XML | 40 | 2 | 0 | 4 |
| Konsol özeti 1 | 40 | 2 | 0 | 4 |
| Konsol özeti 2 | 39 | 2 | 0 | 3 |

İlk konsol özeti XML raporuyla eşleşirken ikinci özet farklı sonuçlar içeriyor.

**Agent'ın davranışı:**
- Build verilerini otomatik olarak topladı.
- Birden fazla konsol test özeti bulunduğunu tespit etti.
- Mevcut sonuçlar arasındaki tutarsızlığı belirledi.
- OpenAI API çağrısını yapmadan süreci durdurdu.

Bu örnek önemli bir tasarım kararını gösteriyor: Agent, kaynaklardan birini diğer kaynakla eşleştiği için keyfî biçimde seçip çelişen kanıtı görmezden gelmiyor.

Tutarsızlığın nedeni henüz kesin olarak açıklanmadı. Özetlerin neden farklı olduğunu ve doğrulama kurallarının bu durumu nasıl ele alması gerektiğini belirlemek için ek inceleme gerekiyor.

### Örnek Hata Senaryoları

Önceki incelemelerde şu tür sorunlarla karşılaşıldı:

- Login bağlantısının tıklanabilir olmasını beklerken zaman aşımı oluşması.
- Arama testinde beklenen `Search - iPhone` sayfa başlığı yerine `My Account` başlığının alınması.
- XML raporları ile Jenkins konsol özetlerinde farklı test sayılarının görülmesi.

Bu gözlemler, incelemenin hangi yönde ilerleyebileceğini gösterir; ancak tek başlarına hatanın kesin nedenini kanıtlamaz. Kök nedeni doğrulamak için ek kanıt gerekir.

### Kullanılan Teknolojiler

- Python
- OpenAI API
- Jenkins REST API
- Selenium WebDriver
- TestNG
- Maven
- Maven Surefire XML raporları
- `xml.etree.ElementTree` ile XML ayrıştırma

### Mühendislik Yaklaşımı

- **Analizden önce doğrulama:** Verileri yapay zekâya göndermeden önce mevcut kanıtları kontrol et.
- **Varsayım yerine kanıt:** Gözlemlenen gerçeklerle olası açıklamaları birbirinden ayır.
- **Güvenli biçimde dur:** Veriler doğrulanamıyorsa süreci durdur.
- **Keyfî kaynak seçimi yok:** Bir raporun başka bir kaynakla eşleşmesi, tek başına onun doğru olduğunu kanıtlamaz.
- **Belirsizliği koru:** Tutarsızlık, araştırılması gereken bir bulgudur; belirli bir kök nedenin kanıtı değildir.
- **İnsan değerlendirmesini koru:** Yapay zekâyı QA mühendisinin yerine geçen bir sistem olarak değil, incelemeyi destekleyen bir yardımcı olarak kullan.
- **Aşamalı geliştirme:** İş akışını genişletmeden önce her bileşeni geliştir, test et ve doğrula.

### Projenin Mevcut Durumu

Proje şu anda Jenkins build'ini otomatik bulmayı, konsol logu ve Surefire XML raporunu toplamayı, test raporlarını ayrıştırmayı, başlangıç seviyesinde tutarlılık kontrolü yapmayı ve koşullu yapay zekâ analizini desteklemektedir.

Veri doğrulama mekanizması geliştirilmeye devam etmektedir. Build #16'da agent, iki farklı konsol test özeti tespit etti ve mevcut doğrulama kuralları gereği OpenAI isteğini doğru şekilde engelledi.

Test sayılarının eşleşmesi, iki dosyanın aynı build'e ait olduğunu veya aynı test çalıştırma kapsamını temsil ettiğini tek başına kanıtlamaz. Raporların nasıl üretildiğini anlamak, build kimliğini ve raporların kaynağını daha güçlü biçimde doğrulamak projenin önemli geliştirme alanlarıdır.

### Planlanan Geliştirmeler

- Aynı Jenkins konsol logunda birden fazla test özetinin neden bulunduğunu araştırmak.
- Build yaşam döngüsü ve rapor oluşturma sürecinden elde edilen kanıtlarla doğrulama kurallarını geliştirmek.
- Build kimliği ve rapor kaynağı doğrulamasını güçlendirmek.
- Word formatında yapılandırılmış inceleme raporları oluşturmak.
- Test sonuçlarını ve hata eğilimlerini gösteren bir dashboard geliştirmek.
- Jenkins build'lerini otomatik tetiklemeyi araştırmak.
- Hataları kategorilere ayırmayı ve sorun giderme önerilerini geliştirmek.
- Kanıta dayalı, kontrollü olası kod düzeltme önerilerini araştırmak.

### Projenin Amacı

Amacım; otomatik test verisi toplamayı, rapor doğrulamayı ve yapay zekâ destekli hata incelemesini birleştiren, ancak kanıtları, belirsizliği, veri tutarlılığını ve insan değerlendirmesini sürecin merkezinde tutan kullanışlı bir QA yardımcısı geliştirmektir.
