```xml
    <resultMap id="Map아이디" type="egovMap">
		<result property="프로퍼티_이름" column="쿼리결과의컬럼명" jdbcType="CLOB" javaType="java.lang.String" />
	</resultMap>

	<select id="쿼리아이디" parameterType="HashMap" resultType="egovMap" resultMap="Map아이디">
		SELECT
			string으로_불러올_clob데이터
		FROM
			테이블
	</select>
```

