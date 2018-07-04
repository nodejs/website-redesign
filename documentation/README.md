# Documentation RFCs

While the he Website Redesign working group continues to iterate on site design and content structure, we will use an RFC process to accept and evaluate Node.js documentation and guide proposals in this repo. Once the new site architecture and documentation ingestion flow is finalized, all docs content will be migrated to its final location and this process updated accordingly.

## When you need to follow this process

You need to follow this process if you intend to make a "substantial" addition or change to the proposed Node.js getting started guides in this repo. What constitutes a "substantial" change evolves based on community norms. Some changes do not require an RFC. These may include reasonably sized re-phasing, reorganizing, or refactoring an existing documentation page.

If you submit a pull request to implement a significant change to the documentation without going through the RFC process, it may be closed with a polite request to submit an RFC first!

## To begin drafting a documentation page:

 1. Fork the [nodejs/website-redesign repo](https://github.com/nodejs/website-redesign/issues).
 2. Copy `getting-started-docs/0000-template.md` to `getting-started-docs/0000-my-article-name.md` (where 'my-article-name' is descriptive. Don't assign the page a number yet).
 3. Fill in the RFC template. Put care into the details! Please review the proposed [Node.js Voice and Tone Guidelines](https://github.com/nodejs/website-redesign/blob/master/style-guide/0001-voice-and-tone.md) before you begin writing.
 4. Submit a pull request. As a pull request the draft will receive feedback from the larger community, and the author should be prepared to revise it in response.
 5. Build consensus and integrate feedback.
 6. Eventually, the Website Redesign Working Group will decide in one of their bi-weekly meetings whether the RFC is a candidate for inclusion on the Node.js website.

RFCs that are candidates for inclusion on the Node.js website will enter a "final comment period" lasting 7 days. The beginning of this period will be signaled with a comment and tag on the RFC's pull request. The RFC can be modified based upon feedback from the community. Significant modifications may trigger a new final comment period. At the close of this review period, the RFC may be:

 * accepted at the close of its final comment period. A working group member will merge the RFC's associated pull request, at which point the RFC will become 'active', or;

 * rejected by the Website Redesign working group after public discussion has settled and comments have been made summarizing the rationale for rejection. A member of the working group should then close the RFC's associated pull request.
