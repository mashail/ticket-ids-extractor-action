# Ticket IDs Extractor Action 🔎
---

This Action will help you extract the task management ticket IDs regardless of whether your task management system is Linear, Jira, or something else.
You need to provide it with the prefix example `PRJ` and whether to be case-sensitive or not, and it will look for the ticket IDs in the PR title, the branch name, the commits messages, and commits bodies if the event is pull_request. and If the event is pushed it will inspect the branch name the commits message and the commit body.
The action will collect all ticket IDs and output it for you in recent_ticket_id_list

## How it looks for the ticket IDs
The action will match the ticket ID with this regex `\s+[A-Z-a-z]-[0-9]+\s+`, it will first replace `[A-Z-a-z]` with the id prefix id_prefix, it does require a minimum of one whitespace before and after to recognize it. It will collect all IDs found based on the event and then output it in `ticket_id_list` to use next in your workflow.

## Inputs

| Input Name  | Description                                      | Required | Default |
|-------------|--------------------------------------------------|----------|---------|
| id_prefix   | Ticket ID prefix to extract                      | Yes      |         |
| insensitive | whethr to match case sensitively or insensitively | No       | true    |

## Outputs

| Output Name           | Description                                                    |
|-----------------------|----------------------------------------------------------------|
| ticket_id_list | Array of the Ticket IDs found that matches the prefix provided |

## Usage Example

Here is an example of how to use it:

```yaml
name: Ticket IDs Extractor
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@692973e3d937129bcbf40652eb9f2f61becf3332 # v4.1.7
      
    - name: Ticket IDs Extractor
      id: ticket_id_extractor
      uses: mashail/ticket-ids-extractor-action@v1.0.0
      with:
        id_prefix: 'PRJ'
        insensitive: 'true'
      
    - name: Print The Ticket Jira ID
      run: echo "--INFO:Here is the ticket ID:${{ steps.ticket_id_extractor.outputs.ticket_id_list }}"
```
