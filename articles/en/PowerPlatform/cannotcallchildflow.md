---
layout: page
title: 'Cannot call a child flow'
menubar: docs_menu
image: 'https://unsplash.com/s/photos/random'
hero_image: '/img/IMG_20220521_140146.jpg'
show_sidebar: false
hero_height: is-small
date: '2025-07-12'
---

# Intro

This article lists reasons why you may not be able to call a child flow and how to fix them.

<br/><br/>

# Because child workflows only support embedded connections

### Error message

Cannot call a child flow: Workflow cannot be used as a child workflow because child workflows only support embedded connections

### Explanation

As per  [Create child flows](https://learn.microsoft.com/en-us/power-automate/create-child-flows)

At this time, you can't pass connections from the parent flow to the child flow. If you don't do this, you receive an error that states that the name cannot be used as a child workflow because child workflows only support embedded connections.


### Solution

If your flow uses anything other than built-in actions or the Microsoft Dataverse connector, you need to update the flow to use the connections embedded in the flow. To do this, go to the child flow's properties page, and then select Edit in the Run only users tile.

<img src="/articles/img/paembeddedconnections.png" width="800">

In the pane that appears, for each connection used in the flow, you will need to select <b>Use this connection</b> instead of <b>Provided by run-only user</b>.

<img src="/articles/img/paembeddedconnections2.png" width="500">


<br/><br/>


# Child flow not visible

If you do not see the child flow in the dropdown of the action "Run child flow":

1. **Make sure you have the permissions to open the child flow.**

Can you open the flow with your account?

2. **Make sure the flow is in the same solution as parent.** If not, add the existing flow to the solution.

Open the parent flow solution. Do you the child flow there?

3. **Make sure the flow has an appropriate trigger.**

The only supported trigger for a child flow is *Manually trigger a flow*.


<br/><br/>

# Parameters mismatch

### Error message

The input body for trigger 'manual' of type 'Request' did not match its schema definition. Error details: 'Required properties are missing from object: location.'.

<img src="/articles/img/pa_childflow2.png" width="800">

### Explanation

The child flow has required parameters that were not supplied.

### Solution

Fill out the required parameters. If the child flow has changed, re-add the action to the parent flow to refresh parameters.

<br/><br/>

# Surprise Extra Parameters for a Child Flow

There is a bug with Power Automate where extra parameters can just appear. If you remove them and correct the child flow - they will reappear again some time later. For me in spring 2025 they kept reappearing every Thursday :)

<img src="/articles/img/pa_childflow.png" width="800">

You can either fill them out and leave ugly like this, or use a nice solution provided by Ian Grieve here:
[Working with Power Automate Child Flows: Error Encountered With Surprise Extra Parameters for a Child Flow](https://www.azurecurve.co.uk/2024/03/working-with-power-automate-child-flows-error-encountered-with-surprise-extra-parameters-for-a-child-flow/)


<br/><br/>


# See Also

[Create child flows](https://learn.microsoft.com/en-us/power-automate/create-child-flows)