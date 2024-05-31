# Cách viết prompt chuẩn (9 tips)
Thành phần cơ bản của prompt:
+ Instruction: Yêu cầu cụ thể bạn muốn AI làm gì
+ Context: Các thông tin bổ sung, ngữ cảnh liên quan
+ Input data: Dữ liệu, thông tin, các dữ kiện mà bạn đang có liên quan đến vấn đề cần giải quyết
+ Output indicator: Format kết quả mà bạn mong muốn
## Tips 1: Cung cấp đầy đủ thông tin và ngữ cảnh
Ví dụ:
+ Create 5 unique call-to-action titles for articles
“content Marketing trend”. The target audience
is marketing expert in Vietnam, article must
have SEO friendly titles related to content
marketing.
+ Dưới đây là một báo cáo doanh thu trong quý 1,
tôi cần bạn tóm tắt lại các điểm chính trong
báo cáo, chỉ cần các điểm chính về doanh thu
để tôi gửi cho sếp
+ Summarize the meeting notes in a single
paragraph. Then write a markdown list of the
speakers and each of their key points. Finally,
list the next steps or action items suggested by
the speakers, if any.

Hãy hình dung ra kết quả bạn muốn nhận được từ đó suy ra mình cần cung cấp thông tin gì, trong ngữ cảnh gì để AI trả lời nhanh hơn và đúng hơn.
## Tips 2: Cá nhân hóa AI cho một việc cụ thể

Your are HR Manager working in {{job_industry}} industry, write a detail
job description for position {{job_name}}, with following information:
{{job_requirement}}

Có nhiều cách để có cá nhân hóa tính cách/kiến thức như:
+ You are.....
+ Pretend you working in...
+ Tên một người cụ thể

## Tips 3: Đánh dấu các thông tin rõ ràng, thêm dấu phần cách
Please proofread for spelling, grammar below text delimited by triple quotes.

“““Advanced AI models hold the promise of
tremendous benefits for humanity, but
society needs to proactively manage the
accompanying risks. In this paper, we focus
on what we term “frontier AI” models: highly
capable foundation models that could
possess dangerous capabilities sufficient to
pose severe risks to public safety“““
## Tips 4: Mô tả các step mà AI nên làm
Perform the following actions:
1 - Summarize the following text delimited by triple
backticks with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
Separate your answers with line breaks.

Text:
```{text here}```

## Tips 5: Cung cấp ví dụ
Viết cho đoạn giới thiệu cho bài viết về {{topic}} với giọng văn giống ví dụ dưới đây:

Ví dụ về giọng văn: {{text}}
## Tips 6: Few-shot Prompting
Là bạn đưa một vài ví dụ bao gồm input mẫu và output mẫu, LLMs sẽ tiếp tục dự đoán output cho input mới
## Tips 7: Ghi rõ kết quả mong muốn cụ thể
Thường là:
+ Độ dài xx
+ Đoạn văn xx
+ xx Bullet points
+ Markdown format
+ HTML format
## Tips 8: Yêu cầu AI xác nhận lại yêu cầu
{task bạn yêu cầu}
Please confirm that you understand my request

Say yes if you understand my request

{task bạn yêu cầu}
Before you perform above request, please summary my quest to make sure you understand it
## Tips 9: Re-Evaluation (tự đánh giá)
Sau khi có một task hoàn chỉnh từ AI, đừng vội dụng. Hãy dùng 1 loop chat mới, với full nội dung AI cũ đã làm và yêu cầu AI mới đánh giá/check lại
# Fact bạn cần hiểu về LLMs
## Fact 1 : Giới hạn về context window
## Fact 2 : Giới hạn về độ dài output (trả kết quả)
## Fact 3 : Giới hạn về bộ nhớ tạm thời
