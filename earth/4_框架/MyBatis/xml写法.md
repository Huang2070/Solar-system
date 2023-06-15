# 根据不同条件执行不同的SQL
``` xml
<where>
    <if test="ispStatus != null">
        <choose>
            <!-- 未注入 -->
            <when test="ispStatus == -1">
                AND distribute.id is null
            </when>
            <!-- 注入中 -->
            <when test="ispStatus == 0">
                AND distribute.process_status in (0,1,2) AND distribute.publish_status = 0
            </when>
            <!-- 注入成功 -->
            <when test="ispStatus == 1">
                AND distribute.process_status = 2 AND distribute.publish_status = 1
            </when>
            <!-- 注入失败 -->
            <when test="ispStatus == 2">
                AND (distribute.process_status = 3 OR distribute.publish_status = 2)
            </when>
        </choose>
    </if>
</where>


```