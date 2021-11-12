# PG建表

```
--创建序列
CREATE SEQUENCE iso_code_seq
START WITH 1
INCREMENT BY 1
NO MINVALUE
NO MAXVALUE
CACHE 1;
--创建表
CREATE TABLE "public"."iso_code" (
  "id" int8  NOT NULL DEFAULT nextval('iso_code_seq'::regclass),
  "code" varchar(255) COLLATE "pg_catalog"."default",
  "name" varchar(255) COLLATE "pg_catalog"."default",
  "cn_name" varchar(255) COLLATE "pg_catalog"."default",
  "type" varchar(255) COLLATE "pg_catalog"."default",
  "old_code" varchar(255) COLLATE "pg_catalog"."default",
  "create_time" timestamp(6) DEFAULT now(),
  "update_time" timestamp(6) DEFAULT now(),
  CONSTRAINT "iso_codepkey" PRIMARY KEY ("id")
);
ALTER TABLE "public"."iso_code" 
  OWNER TO "yxjava01";
COMMENT ON COLUMN "public"."iso_code"."id" IS '序列id编号';
COMMENT ON COLUMN "public"."iso_code"."code" IS 'ISO 3166-2 新对照码';
COMMENT ON COLUMN "public"."iso_code"."name" IS '地区拼音名称';
COMMENT ON COLUMN "public"."iso_code"."cn_name" IS '中文地区名称';
COMMENT ON COLUMN "public"."iso_code"."type" IS '行政区类型';
COMMENT ON COLUMN "public"."iso_code"."old_code" IS 'ISO 3166-2 旧对照码';
COMMENT ON COLUMN "public"."iso_code"."create_time" IS '创建时间';
COMMENT ON COLUMN "public"."iso_code"."update_time" IS '修改时间';
COMMENT ON TABLE "public"."iso_code" IS 'ISO 3166-2 新对照码表';

```

# HIVE建表

```
drop table if exists yxsj.dim_iso_code;
create external table yxsj.dim_iso_code( 
`id` int COMMENT "主键id"
,`code` string COMMENT "iso code码"
,`name` string COMMENT "行政区拼音"
,`cn_name` string COMMENT "行政区中文名称"
,`type` string COMMENT "行政区类型"
,`old_code` string COMMENT "旧版isocode码"
,`create_time` timestamp COMMENT "创建时间"
,`update_time` timestamp COMMENT "修改时间"
) COMMENT "设备详情表"
row format delimited fields terminated by '\t' 
STORED AS
INPUTFORMAT 'com.hadoop.mapred.DeprecatedLzoTextInputFormat'
OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
location '/user/hive/warehouse/yxsj/dim/dim_iso_code/';

```

