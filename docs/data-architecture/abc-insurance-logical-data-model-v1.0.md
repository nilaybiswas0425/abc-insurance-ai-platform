# ABC Insurance -- Logical Data Model (DBML)

## Purpose

This document contains the logical database model for the ABC Insurance
-- Enterprise Claims Intelligence Platform, expressed in DBML for use
with [dbdiagram.io](https://dbdiagram.io/).

## Tables and Relationships

``` text

Table CUSTOMER {
  customer_id number(32) [primary key]
  customer_first_name varchar2(250)
  customer_middle_name varchar2(250)
  customer_last_name varchar2(250)
  age number(32,0)
  customer_address varchar2(250)
  customer_city varchar2(250)
  customer_postal_code varchar2(250)
  customer_country varchar2(250)
  created_at datetime
  updated_at datetime
}

Table POLICY {
  policy_id number(32) [primary key]
  customer_id number(32) [not null]
  policy_number varchar2(100) [not null, unique]
  policy_type varchar2(100)
  policy_start_date datetime
  policy_end_date datetime
  premium_amount number(18,2)
  coverage_amount number(18,2)
  policy_status varchar2(50)
  created_at datetime
  updated_at datetime
}

Table CLAIM {
  claim_id number(32) [primary key]
  policy_id number(32) [not null]
  claim_number varchar2(100) [not null, unique]
  claim_date datetime
  reported_date datetime
  incident_date datetime
  incident_description varchar2(2000)
  claimed_amount number(18,2)
  created_at datetime
  updated_at datetime
}

Table CLAIM_DOCUMENT {
  document_id number(32) [primary key]
  claim_id number(32) [not null]
  document_type_id number(32) [not null]
  document_name varchar2(250)
  document_description varchar2(1000)
  document_version number(10,0) [not null]
  is_latest_version varchar2(1) [not null]
  supersedes_document_id number(32)
  s3_bucket_name varchar2(250)
  s3_object_key varchar2(1000) [not null]
  document_format varchar2(50)
  uploaded_at datetime
  processing_status varchar2(50)
  created_at datetime

  indexes {
    (claim_id, document_type_id, document_version) [unique]
  }
}

Table CLAIM_INVESTIGATION {
  investigation_id number(32) [primary key]
  claim_id number(32) [not null]
  investigator_name varchar2(250)
  investigation_type varchar2(100)
  investigation_notes varchar2(2000)
  investigation_status varchar2(50)
  investigation_start_date datetime
  investigation_end_date datetime
  created_at datetime
}

Table CLAIM_ASSESSMENT {
  assessment_id number(32) [primary key]
  claim_id number(32) [not null]
  assessment_version number(10,0) [not null]
  assessed_amount number(18,2)
  assessment_notes varchar2(2000)
  assessed_by varchar2(250)
  assessment_date datetime
  created_at datetime

  indexes {
    (claim_id, assessment_version) [unique]
  }
}

Table CLAIM_DECISION {
  decision_id number(32) [primary key]
  claim_id number(32) [not null]
  decision_type varchar2(50)
  decision_status varchar2(50)
  decision_reason varchar2(2000)
  decision_amount number(18,2)
  decided_by varchar2(250)
  decision_date datetime
  created_at datetime
  updated_at datetime
}

Table PAYMENT {
  payment_id number(32) [primary key]
  claim_id number(32) [not null]
  payment_status_id number(32) [not null]
  payment_reference_number varchar2(100) [unique]
  payment_amount number(18,2)
  payment_date datetime
  payment_method varchar2(50)
  created_at datetime
  updated_at datetime
}

Table CLAIM_STATUS {
  claim_status_id number(32) [primary key]
  claim_id number(32) [not null]
  claim_status varchar2(50) [not null]
  source_effective_start_timestamp datetime [not null]
  source_effective_end_timestamp datetime
  current_row_indicator varchar2(1) [not null]
  status_changed_by varchar2(250)
  status_change_reason varchar2(1000)
  created_at datetime
  updated_at datetime
}

Table DOCUMENT_TYPE {
  document_type_id number(32) [primary key]
  document_type_code varchar2(50) [not null, unique]
  document_type_name varchar2(250)
  document_description varchar2(500)
  active_indicator varchar2(1)
}

Table PAYMENT_STATUS {
  payment_status_id number(32) [primary key]
  payment_status_code varchar2(50) [not null, unique]
  payment_status_name varchar2(250)
  payment_status_description varchar2(500)
  active_indicator varchar2(1)
}

// CUSTOMER to POLICY: One-to-many
Ref: POLICY.customer_id > CUSTOMER.customer_id

// POLICY to CLAIM: One-to-many
Ref: CLAIM.policy_id > POLICY.policy_id

// CLAIM to CLAIM_DOCUMENT: One-to-many
Ref: CLAIM_DOCUMENT.claim_id > CLAIM.claim_id

// DOCUMENT_TYPE to CLAIM_DOCUMENT: One-to-many
Ref: CLAIM_DOCUMENT.document_type_id > DOCUMENT_TYPE.document_type_id

// Previous document version
Ref: CLAIM_DOCUMENT.supersedes_document_id > CLAIM_DOCUMENT.document_id

// CLAIM to CLAIM_INVESTIGATION: One-to-many
Ref: CLAIM_INVESTIGATION.claim_id > CLAIM.claim_id

// CLAIM to CLAIM_ASSESSMENT: One-to-many
Ref: CLAIM_ASSESSMENT.claim_id > CLAIM.claim_id

// CLAIM to CLAIM_DECISION: One-to-many
Ref: CLAIM_DECISION.claim_id > CLAIM.claim_id

// CLAIM to PAYMENT: One-to-many
Ref: PAYMENT.claim_id > CLAIM.claim_id

// PAYMENT_STATUS to PAYMENT: One-to-many
Ref: PAYMENT.payment_status_id > PAYMENT_STATUS.payment_status_id

// CLAIM to CLAIM_STATUS: One-to-many
Ref: CLAIM_STATUS.claim_id > CLAIM.claim_id

```

## Usage

1.  Open [dbdiagram.io](https://dbdiagram.io/).
2.  Create a new diagram.
3.  Copy the DBML code block above into the editor.
4.  Review the generated entity relationship diagram.
