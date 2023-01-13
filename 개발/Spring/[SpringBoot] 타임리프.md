**JpaRepository**

디비사용코드 https://wikidocs.net/160890

# 타임리프

[참고](https://wikidocs.net/161186)

``` html
<table>
	<thead>
		<tr>
			<th>제목</th>
			<th>작성일자</th>
		</tr>
	</thead>
	<tbody>
		<tr th:each="question: %{questionList}">
			<td th:text="${question.subject}"></td>
			<td th:text="${question.createDate}"></td>
		</tr>
	</tbody>
</table>
```

-  `th:` 로 시작하는 속성은 타임리프 템플릿 엔진이 사용하는 속성