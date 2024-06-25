The folder contains the original documents.

* Disney = https://thewaltdisneycompany.com/app/uploads/2024/05/q2-fy24-earnings-transcript.pdf
* Netflix = https://s22.q4cdn.com/959853165/files/doc_downloads/2024/4/netflix-inc-_earnings-call_2024-04-18_english.pdf
* Kraft Heinz = https://ir.kraftheinzcompany.com/static-files/22b797ca-145a-4a4c-9360-e5d0ddeb80c8
* Choice Hotels International = https://s201.q4cdn.com/538915302/files/doc_financials/2024/q1/q1-transcript.pdf
* Apple = https://seekingalpha.com/article/4688870-apple-inc-aapl-q2-2024-earnings-call-transcript

PDFs are converted into text using `pdftotext` command-line tool.

Notes:
- the Netflix txt file contains some weird character that had to removed manually. It was crashing the `llm` tool.
- ditto for the Disney file.