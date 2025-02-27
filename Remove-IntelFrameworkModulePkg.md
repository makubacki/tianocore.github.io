Below is the directory tree of IntelFrameworkModulePkg.
The link after the directory name is the GitHub issue link tracking the removal of it.

<pre>
IntelFrameworkModulePkg
	+---Bus
	|   +---Isa                <a href="https://github.com/tianocore/edk2/issues/7625">[7625]</a>
	|   \---Pci 
	|       +---IdeBusDxe      <a href="https://github.com/tianocore/edk2/issues/7639">[7639]</a>
	|       \---VgaMiniPortDxe <a href="https://github.com/tianocore/edk2/issues/7626">[7626]</a>
	+---Csm
	|   +---BiosThunk
	|   |   +---BlockIoDxe
	|   |   +---KeyboardDxe
	|   |   +---Snp16Dxe
	|   |   \---VideoDxe
	|   \---LegacyBiosDxe
	+---Include
	|   +---Guid
	|   +---Library
	|   \---Protocol
	+---Library
	|   +---BaseUefiTianoCustomDecompressLib ->Liming, propose drop
	|   +---DxeCapsuleLib                             <a href="https://github.com/tianocore/edk2/issues/7642">[7642]</a>
	|   +---DxeReportStatusCodeLibFramework           <a href="https://github.com/tianocore/edk2/issues/7628">[7628]</a>
	|   +---GenericBdsLib                             <a href="https://github.com/tianocore/edk2/issues/7624">[7624]</a>
	|   +---LegacyBootMaintUiLib
	|   +---LegacyBootManagerLib
	|   +---LzmaCustomDecompressLib                   <a href="https://github.com/tianocore/edk2/issues/7640">[7640]</a>
	|   +---PeiDxeDebugLibReportStatusCode            <a href="https://github.com/tianocore/edk2/issues/7628">[7628]</a>
	|   +---PeiRecoveryLib                            <a href="https://github.com/tianocore/edk2/issues/7610">[7610]</a>
	|   +---PeiS3Lib                                  <a href="https://github.com/tianocore/edk2/issues/7610">[7610]</a>
	|   +---PlatformBdsLibNull                        <a href="https://github.com/tianocore/edk2/issues/7624">[7624]</a>
	|   \---SmmRuntimeDxeReportStatusCodeLibFramework <a href="https://github.com/tianocore/edk2/issues/7628">[7628]</a>
	\---Universal
	    +---Acpi
	    |   +---AcpiS3SaveDxe                         <a href="https://github.com/tianocore/edk2/issues/7652">[7652]</a>
	    |   \---AcpiSupportDxe                        <a href="https://github.com/tianocore/edk2/issues/7648">[7648]</a>
	    +---BdsDxe                                    <a href="https://github.com/tianocore/edk2/issues/7624">[7624]</a>
	    +---Console\VgaClassDxe                       <a href="https://github.com/tianocore/edk2/issues/7626">[7626]</a>
	    +---CpuIoDxe                                  <a href="https://github.com/tianocore/edk2/issues/7644">[7644]</a>
	    +---DataHubDxe                                <a href="https://github.com/tianocore/edk2/issues/7628">[7628]</a>
	    +---DataHubStdErrDxe                          <a href="https://github.com/tianocore/edk2/issues/7628">[7628]</a>
	    +---FirmwareVolume
	    |   +---FwVolDxe                              <a href="https://github.com/tianocore/edk2/issues/7647">[7647]</a>
	    |   \---UpdateDriverDxe                       <a href="https://github.com/tianocore/edk2/issues/7646">[7646]</a>
	    +---LegacyRegionDxe                           <a href="https://github.com/tianocore/edk2/issues/7645">[7645]</a>
	    +---SectionExtractionDxe                      <a href="https://github.com/tianocore/edk2/issues/7647">[7647]</a>
	    \---StatusCode                                <a href="https://github.com/tianocore/edk2/issues/7628">[7628]</a>
	        +---DatahubStatusCodeHandlerDxe
	        +---Pei
        \---RuntimeDxe
</pre>