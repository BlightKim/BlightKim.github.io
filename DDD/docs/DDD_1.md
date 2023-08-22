---
layout: default
title: DDD(도메인 주도 개발)_1.전략 디자인

---

# 비즈니스 도메인 분석
> 코드 작성에 앞서서 회사가 일하는 방식을 알아야 한다.
> 
> - 존재 이유
> - 그들이 추구하는 목적
> - 목적을 달성하기 위한 전략

# 비즈니스 도메인의 정의

A business domain defines a company’s main area of activity. Generally speaking, it’s the service the company provides to its clients. For example:

서브 도메인이란 ?

To achieve its business domain’s goals and targets, a company has to operate in multiple subdomains.
A subdomain is a fine-grained area of business activity.


코어 서브도메인이란 ? 

A core subdomain is what a company does differently from its competitors.
예시 : Let’s take Uber as an example. Initially, the company provided a novel form of transportation: ridesharing. As its competitors caught up, Uber found ways to optimize and evolve its core business: for example, reducing costs by matching riders heading in the same direction.

It’s important to note that core subdomains are not necessarily technical.

Generic subdomains ? 이란 ?

Generic subdomains are business activities that all companies are performing in the same way.

차이는 ? However, generic subdomains do not provide any competitive edge for the company.

예시 : For example, most systems need to authenticate and authorize their users. Instead of inventing a proprietary authentication mechanism, it makes more sense to use an existing solution.