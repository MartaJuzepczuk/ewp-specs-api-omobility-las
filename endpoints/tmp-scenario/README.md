Learning agreement: schema simplification
=========================================

The current version of the learning agreement is quite complicated. We are trying to propose a slightly simpler version that still will be compatible with the Commission's guidelines (https://ec.europa.eu/programmes/erasmus-plus/resources/documents/applicants/learning-agreement_en).

In the current version of the scheme, both `components-studied` (tables A / A2) and `components-recognized` (tables B / B2) consist of six parts:
- `before-mobility-changes`
- `before-mobility-snapshot`
- `latest-approved-changes`
- `latest-approved-snapshot`
- `latest-draft-changes`
- `latest-draft-snapshot`

In short, our suggestion is to remove all `changes` parts and slightly modify the `snapshot` parts, so that we can express the reasons for the changes in LA after it has been first signed (by all three parties - tables A2 and B2).

**Why we want to get rid of the `changes` parts?**
- Keeping the whole history of changes is not required in the LA process.
- Expressing changes in the current version of the API is complicated. Because we do not have any required identifiers or codes for the courses on the receiving hei (`components-studied`), and it seems that we can not require them, we use the position numbers on the list (indexes). When the change consists of several removal and addition operations, the indexes may be interpreted differently. Of course, we have to impose one way of interpretation, but it can still easily lead to errors.
- Not everyone stores the entire change history on the system and the current specification allows it. In such cases, the server is allowed to calculate the `changes` parts based on the current and previous version before sending. It seems unnecessary. Equally, the client can do such a calculation if for some reason he wants to use it.
- Courses lists in LA are not long. Coordinators can (or even may prefere) to view entire, ready versions of LA. Or the client systems, even using poor algorithms, can calculate these changes if needed.

**What we propose**
- We will keep only three parts: `before-mobility-snapshot`, `latest-approved-snapshot` and `latest-draft-snapshot`. All of them seem necessary, because we need to represent Table A and B (`before-mobility-snapshot`), current version (`latest-approved-snapshot`) and we also need a place to send proposals, which have not been signed yet (`latest-draft-snapshot`).
- The `before-mobility-snapshot` part will not be changed.
- In the other two parts, with each course it will be possible to additionally say that, in relation to the "before" version, it has been removed or added, and send the reason of the change. The new fields (inserted/deleted flag and the reason) will be used only when the LA has been already first signed. The flag can be computed based on the courses listed in the `before-mobility-snapshot` and `latest-approved-snapshot`, but we need a place to explain the reason of such a change. 

**Example scenario**
For clarity, the examples contain only information about courses that the student wants to attend at the receiving hei (`components-studied`). Attached files *are not* valid get responses or valid update requests.

* 01-initial-version.xml - first proposal of the LA, already accepted by the student and the sending hei (get response)
* 02-update-components-studied.xml - the receiving hei does not accept the LA and proposes some changes (update request)
* 03-second-version-get.xml - second proposal of the LA, taking into account the comments of the partner, already accepted by the student and the sending hei (get response)
* 04-approve-components-studied-draft.xml - the receiving hei accepts the LA (update request)
* 05-signed-version-before.xml - first version of the LA which is signed by all three parties (before, Table A) (get response)
* 06-changes-during-proposal.xml - student wants to change one course (get response)
* 07-approve-components-studied-draft.xml - the receiving hei accepts the change (update request)
* 08-changes-during-proposal.xml - change applied; student wants to change two more courses - one of them added in previous change (get response)
* 09-update-components-studied.xml  - the receiving hei does not accept the change (update request)
* 10-changes-during-fixed-proposal.xml - student wants to change one more course, taking into account the receiving hei comments (get response)
* 11-approve-components-studied-draft.xml - the receiving hei accepts the change (update request)
* 12-final-version.xml - final version of the LA, with two changes during the mobility (get response) 
