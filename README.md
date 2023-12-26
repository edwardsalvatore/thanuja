<script th:inline="javascript">
    /*<![CDATA[*/
    th:if="${displayAlert}" 
        alert('An error occurred while checking the URLs. Error: ' + /*[(${errorMessage})]*/ '');
    /*]]>*/
</script>
