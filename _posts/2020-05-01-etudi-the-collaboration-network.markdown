---
layout: post
title: Etudi - The Collaboration Network For Medicine Journal Publications
date: 2020-05-01 00:00:00 +0300
description: Side project which retrives biomedical and genomic articles from National Center for Biotechnology Information Databases. # Add post description (optional)
img: from-idea-to-action.jpg # Add image post (optional)
tags: [React, TypeScript, AWS, Python, Software] # add tag
---

Etudi is the collaboration network where academia and the industry meet to discuss ideas, share information, discuss experiments as well as publish these findings in order to drive innovation and significantly improve the quality of the research papers. 

### Where did the idea come from?

In the spring of 2020, I found myself spending a lot of time indoors given the uncertainty of the pandemic. As I was catching up with one of my friends, we were discussing the challenges faced by students when it comes to searching for and writing academic papers. We quickly reached to the conclusion that research departments from both academia and the industry would be more productive if they could share ideas and experiment results in a digital environment that would facilitate publishing as well. That is when the idea of building a collaboration network was born.

![Etudi Welcome Page]({{site.baseurl}}/assets/img/etudi.png)

### Results driven by the Agile Methodology

As the lead and only developer on the project, building a collaboration network from scratch sounded exciting and scary at the same time. Regardless, I rolled up my sleeves and started coding. Time commitment was 12 hours per week on average. Using the [Agile Methodology][agile] in delivering the work helped a lot. My friend and I would have weekly catch-ups on Sundays where we would look at the [Trello Board][trello], discuss the progress and divide the business and tech tasks among ourselves.

4 months later, we built the alpha version of the etudi platform, had a business proposal and received endorsement from academics of the Medical Sciences Division from the Oxford University. The alpha version of the etudi platform consisted of allowing users to quickly search and read scientific papers in a friendly UI by connecting to the [PubMed Database][pubmed] from the [National Center for Biotechnology Information][ncbi]. Users could also create accounts or login using existing credentials. Another feature would allow users to add the articles to a list of favourites for further reference.

### Technology Stack

The front-end was built using React and Redux. With the help of [Babel][babel], one of the most popular compilers responsible with converting ECMAScript 2015+ code into a backwards compatible version of JavaScript in current and older browsers or environment, the code had both JavaScript and TypeScript files. While this is not a best-practice, as it is advised to keep the same language and file type across the project, it invites all developers to contribute to the codebase including those who are less experienced with TypeScript.

The back-end was built using AWS Amplify which allows developers to build full stack serverless apps. The Amplify CLI helps developers to create backend resources through a guided workflow using either Python or NodeJS. On top of this, I created a AWS Lambda layer using Python and Flask which connects to the PubMed database and serves requests from the front-end. A layer is a ZIP archive that contains libraries, a custom runtime, or other dependencies. The 2 major benefits are: code reusability and faster deployments (lambda function can be updated independently within the layer). For more information on how to use lambda layers with the Amplify CLI, please refer to [this article][article]. 

In terms of CI/CD (Continuous Integration / Continuous Deployment), I leveraged the capabilities of the AWS Ampliy which was performing automatic builds and deployments per GitHub branches, hence allowing multiple versions of the same application to run in parallel so that new features could be tested much faster. Everything was done behind the scenes by simply connecting the GitHub repository, no need for a config file! For a complete guide on how to perform these automated workflows, use [the following article][article2].

### Conclusion

Never be afraid to get out of your comfort zone and try something new! I have learnt so much in such a little amount of time: from working with AWSAmplify to understanding how to the London start-up ecosystem works.  


### Github Repos
[Etudi Front-end][etudiweb]

[Etudi Back-end][etudi]


[agile]: https://medium.com/zenkit/agile-methodology-an-overview-7c7e3b398c3d
[trello]: https://trello.com/en-GB/tour
[pubmed]: https://pubmed.ncbi.nlm.nih.gov/
[ncbi]: https://www.ncbi.nlm.nih.gov/
[babel]: https://babeljs.io/
[article]: https://aws.amazon.com/blogs/mobile/how-to-use-lambda-layers-with-the-amplify-cli/
[article2]: https://aws.amazon.com/blogs/mobile/complete-guide-to-full-stack-ci-cd-workflows-with-aws-amplify/
[etudiweb]: https://github.com/andreeaionescu/etudiweb
[etudi]: https://github.com/andreeaionescu/etudi
