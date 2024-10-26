---
title: 디버깅 사용법
layout: default
nav_order: 4.5
---
{: .label .label-red }
디버거는 내가 조사하려는 로직이 무엇인가 ? 를 알 때 사용한다.

### 실행 스택 트레이스

**실행 스택 트레이스** - 디버거가 중단시킨 코드 라인의 실행 경로를 나타낸다.

* 실행 스택은 아래에서 위로 읽는다. 가장 위에 있는 레이어는 현재 실행이 중단된 레이어
* 실행 스택은 aspect / interceptor 등 관심사 분리 기법이 적용된 코드를 디버깅 할 때 효과적이다.


### 디버거 코드 탐색 기본
1. 스텝 오버 - 동일한 메서드에서 다음 코드라인 실행
2. 스텝 인투 - 현재 라인에서 호출된 메서드 하나의 내부에서 계속 실행
3. 스텝 아웃 - 호출한 메서드로 실행을 되돌림


{: .important }
> 디버깅을 시작하기 전에 코드를 먼저 읽어라
>
> 스텝 인투 후 코드가 이해된다면 다시 스텝 아웃해서 코드를 효율적으로 디버깅 한다

[//]: # (You specify the layout for a page in its [front matter]. Just the Docs has a `default` layout with a sidebar, used for almost all pages in the theme docs, and a `minimal` layout that omits the sidebar.)

[//]: # ({: .fs-6 .fw-300 })

[//]: # ()
[//]: # (## The layout concept)

[//]: # ()
[//]: # (See the [Jekyll docs page about layouts] for an explanation of the general idea of layouts and how to specify them.)

[//]: # ()
[//]: # (You can use [Jekyll's front matter defaults] to specify the same layout for many pages.)

[//]: # ()
[//]: # (## The `default` layout)

[//]: # ()
[//]: # (This page uses the default layout. This site configures `layout: default` as a [front matter default]&#40;https://jekyllrb.com/docs/configuration/front-matter-defaults/&#41; value for all pages in the `docs` directory.)

[//]: # ()
[//]: # (The default layout of Just the Docs is a *responsive* layout: on medium and larger width displays, it displays a sidebar, including a navigation panel; on smaller width displays, the sidebar is automatically hidden under a button.)

[//]: # ()
[//]: # (All pages except top-level pages automatically have a list of  so-called *breadcrumbs*: links to their parent pages and any higher-level ancestors. They show the breadcrumbs above the main content of the page.)

[//]: # ()
[//]: # (Each page that has child pages generally has a list of links to those pages &#40;you can suppress it by `has_toc: false` in the front matter&#41;. It shows the list as a *table of contents* below the main content.)

[//]: # ()
[//]: # (## The `minimal` layout)

[//]: # ()
[//]: # (A child and grandchild page of this page use the minimal layout. This differs from the default layout by omitting the sidebar---and thereby also the navigation panel. To navigate between pages with the minimal layout, you can use the breadcrumbs and the tables of contents.)

[//]: # ()
[//]: # (## Selectively hiding or showing the sidebar)

[//]: # ()
[//]: # ([Jekyll's front matter defaults] can be used to apply the `minimal` layout for many pages. But there are also other variables that can control the page layout. In `_config.yml`, you can set `nav_enabled: false` to disable the sidebar navigation panel across the entire site. This can then be selectively enabled on a page-by-page basis by assigning the `nav_enabled: true` page [front matter] variable. For instance, this could be used to enable sidebar navigation on a home page while all other pages have sidebar navigation disabled.)

[//]: # ()
[//]: # (```yaml)

[//]: # (---)

[//]: # (layout: default)

[//]: # (title: Home)

[//]: # (nav_enabled: true)

[//]: # (---)

[//]: # ()
[//]: # (```)

[//]: # ()
[//]: # (## Other layouts)

[//]: # ()
[//]: # (Just the Docs has further layouts: `about`, `home`, `page`, and `post`. Currently, they are all based on the `default` layout. See the [Jekyll docs about inheritance] for how to customize them.)

[//]: # ()
[//]: # ([front matter]: https://jekyllrb.com/docs/front-matter/ "Jekyll docs about front matter")

[//]: # ([Jekyll docs page about layouts]: https://jekyllrb.com/docs/layouts/ "Jekyll docs about layouts")

[//]: # ([Jekyll's front matter defaults]: https://jekyllrb.com/docs/configuration/front-matter-defaults/ "Jekyll docs about front matter defaults")

[//]: # ([Jekyll docs about inheritance]: https://jekyllrb.com/docs/layouts/#inheritance "Jekyll docs about inheritance")
