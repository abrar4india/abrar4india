swagger: "2.0"
info:
  title: FPD IRSF Notification API
  description: "\n# Introduction\n\n
  <table>
    <tbody>
      <tr>
        <td class = 'into_api' style='border:none;padding:0 0 0 0'>
          <p>FPD International Revenue Share Fraud (IRSF) API provide qualified Service Number or IMSI to the robotics team for suspension action.\n\ 
          
          Post the action performed by robotics in MICA and SIEBEL billing system, case update API will be initiated by robotics to provide notification on the outcome of each of the NetMind cases based on correlation id </p>
        </td>
      </tr>
    </tbody>
  </table>\n\n
  # Features\n\ FPD IRSF Notification API provides below features.\n
  | Feature | Description |\n
  |---|---|\n
  |get `requests` | This operation will retrieve all IRSF cases which is NEW,if IMSI or Service Number is not mentioned in request. |\n
  |post `requests` | This operation will update the status of IRSF cases actioned by the receiver system.  |\n
  # Getting Access to the API\n\n\n\n\n\n\n
  Now head over to **Getting Started** where you can find more details about the API.\n\n
  # Getting Started\n\nBelow are the steps to get started with the Telstra Messaging API.\n
  1. Generate an OAuth2 Token using your `Client key` and `Client secret`.\n
  2. Use OAuth Token to Trigger a Request to Messaging API10\n\n\n# Frequently Asked Questions\n\n
  **Q: Which authorization protocol will be used for enabling access to IRSF API?**\n
  A. OAuth2 authorization have been used for enabling access this API.\n\n\n\n
  **Q: What is FPD?**\n
  A. FPD stands for Fraud Prevention and Detection owned by Infogix which is Fraud management partner for Telstra.\n\n
  **Q: How can i get support for this API?**\n
  A. Infogix-Telstra Support TelstraSupport@precisely.com\n\n
  ## Notes\n
  View API documentation \"Getting Started.MD\" and \"Introduction.MD\" for more details.\n"
  version: "1.0.0"
host: "private-tapi.telstra.com"
basePath: "/application/fpd/v1/"
schemes:
- "https"
paths:
  /application/fpd/v1/irsfCases:
    get:
      tags:
      - "Fetch IRSF Cases"
      summary: "Returns qualified cases which is new or not actioned by consuming system"
      description: "Returns all IRSF qualified cases, if no parameter is supplied."
      produces:
      - "application/json"
      parameters:
      - name: Authorization
        in: header
        description: A header in the format 'Bearer {access_token}' - get the token by using the OAuth2.0 API with the scope 'irsf-cases' API. API is Protected with MASSL and client cert need to be presented to access it. 
        type: string
        required: true
      - name: "irsf_request"
        in: body
        description: If in the request body no details are mentioned it will return all the cases which are not marked as COMPLETE or INCOMPLETE in NetMind. For querying based on  IMSI or Service Number for a specific search, supplying both parameters will result in an error.
        schema:
          $ref: "#/definitions/irsfReq"
      responses:
        200:
          description: Request data consumed successfully.
          schema:
            $ref: '#/definitions/irsfReqRes'
        400:
          description: Bad Request
          schema:
            $ref: '#/definitions/FunctionalError'
        401:
          description: Unauthorized
          schema:
            $ref: '#/definitions/FunctionalError'
        403:
          description: Forbidden
          schema:
            $ref: '#/definitions/FunctionalError'
        404:
          description: Not Found
          schema:
            $ref: '#/definitions/FunctionalError'
        412:
          description: Bad Request
          schema:
            $ref: '#/definitions/Error_412'
        500:
          description: System Error, retry after some time. Please contact FPDS support group if problem exists.          
          schema: 
            $ref: '#/definitions/Error_500'

  /application/fpd/v1/updateCaseNote:
    post:
      tags:
      - "Update IRSF Case Notes"
      summary: "Update IRSF Case Notes"
      description: "Update IRSF Case Notes"
      produces:
      - "application/json"
      parameters:
      - in: "body"
        name: "body"
        description: "To update case note"
        required: true
        schema:
          $ref: "#/definitions/updateCaseNoteReq"
      responses:
        200:
          description: "successful operation"
          schema:
            $ref: "#/definitions/updateCaseNoteRes"
        400:
          description: Bad Request
          schema:
            $ref: '#/definitions/FunctionalError'
        401:
          description: Unauthorized
          schema:
            $ref: '#/definitions/FunctionalError'
        403:
          description: Forbidden
          schema:
            $ref: '#/definitions/FunctionalError'
        404:
          description: Not Found
          schema:
            $ref: '#/definitions/FunctionalError'
        412:
          description: Bad Request
          schema:
            $ref: '#/definitions/Error_412_Update'
        500:
          description: System Error, retry after some time. Please contact FPDS support group if problem exists.          
          schema: 
            $ref: '#/definitions/Error_500'

definitions:
  irsfReq:
    type: "object"
    properties:
      imsi:
        type: "string"
        example: "505000123456789"
      serviceNumber:
        type: "string"
        example: "0498764567"
  irsfReqRes:
    type: "object"
    properties:
      timestamp:
        type: "string"
        example: "2018-12-13T15:53:00+11:00"
      message:
        type: "string"
        example: "success"
      cases:
        type: array
        items:
          $ref: '#/definitions/irsfReqResItem'
  irsfReqResItem:
    type: "object"
    properties:
      correlationId:
        type: "string"
        example: "fpd-08232021-061400-1"    
      billingSystem:
        type: "string"
        example: "MICA"
      serviceNumber:
        type: "string"
        example: "0498764567"
      imsi:
        type: "string"
        example: "505000123456789"
      fpdCaseNo:
        type: "string"
        example: "432121"
  Error_412:
    type: "object"
    properties:
      timestamp:
        type: "string"
        example: "2018-12-13T15:53:00+11:00"
      correlationId:
        type: "string"
        example: "fpd-08232021-061400-1"
      message:
        type: "string"
        example: "Provide one parameter imsi or service number and try again."
  Error_412_Update:
    type: "object"
    properties:
      timestamp:
        type: "string"
        example: "2018-12-13T15:53:00+11:00"
      correlationId:
        type: "string"
        example: "fpd-08232021-061400-1"
      message:
        type: "string"
        example: "Scenario1: Required field missing.
                  Scenario2: Message length is greater than specified length of 50 characters"
  Error_500:
    type: "object"
    properties:
      timestamp:
        type: "string"
        example: "2018-12-13T15:53:00+11:00"
      correlationId:
        type: "string"
        example: "fpd-08232021-061400-1"
      code:
        type: "number"
        example: "3001"
      message:
        type: "string"
        example: "Internal Server Error"
  updateCaseNoteReq:
    type: "object"
    properties:
      timestamp:
        type: "string"
        example: "2018-12-13T15:53:00+11:00"
      correlationId:
        type: "string"
        example: "fpd-08232021-061400-1"  
      fpdCaseNo:
        type: "string"
        example: "432121"
      message:
        type: "string"
        example: "Suspension completed successfully"
      suspensionStatus:
        type: "number"
        example: "values 0: suspension completed,1: suspension in-progress"
  updateCaseNoteRes:
    type: "object"
    properties:
      timestamp:
        type: "string"
        example: "2018-12-13T15:53:00+11:00"
      correlationId:
        type: "string"
        example: "fpd-08232021-061400-1"  
      message:
        type: "string"
        example: "success"
  FunctionalError:
    title: FunctionalError
    type: object
    properties:
      message:
        description: Error message
        type: string
      error_description:
        description: OAuth Error message
        type: string
    required:
      - message
