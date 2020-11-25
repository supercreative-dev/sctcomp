# sctcomp
SuperCreative Texture Compressor

sctcomp는 google/etc2comp로부터 파생 된 커맨드라인 툴입니다. sct포멧이지만 베이스는 [ETC2](https://en.wikipedia.org/wiki/Ericsson_Texture_Compression) 입니다. (구)sct는 사라질 예정이고 ect2 베이스의 sct 포멧만 남을 예정입니다.

## 설치(Windows MSVS)
1. 적당한 폴더를 만듭니다(아래)
1. `mkdir build_vs`
1. `cd build_vs`
1. 그런 다음 CMAKE를 실행시켜 주세요.
  VS 2019 : `cmake -G "Visual Studio 16 2019" -A "win32" ../`
  주의: 현재 사용하는 버전을 확인하고 싶으시면 다음의 명령어를 입력해주세요. `cmake -G`
1. 'SctCompBuild' 솔루션을 연다음,
1. 'sctcomp' 프로젝트를 start up 프로젝트로 설정하고,
1. (optional) sctcomp 프로젝트 설정에서, 'Debugging ->command arguments'에 가셔서 명령어를 입력하여 테스트 할 수 있습니다.
example: -argfile C:\etc2\EtcTool\Args.txt

주의 : EtcTool/CMakeLists.txt에 추가해둔 헤더, 라이브러리, 소스파일은 c:\scttool와 c:\Repos 기준으로 추가되어 있습니다. 본인만의 경로에 맞춰서 수정해줄 필요 있습니다!!
주의 : 빌드가 잘안되면 '조화평'을 불러주세요.


## Usage

### Command Line Usage
sctcomp는 커맨드라인 툴이기 때문에 실행인자로 기능을 제공합니다.
Python 스크립트를 통해 텍스쳐 컨버터를 사용하도록 할 예정입니다.
(핵심 옵션 및 커스텀 옵션만 번역해둡니다..)

etctool.exe source_image [options ...] -output encoded_image

Options:

    -analyze <analysis_folder>    컨버팅한 결과물 텍스쳐와 블록정보를 이미지화 시킨 텍스쳐를 만들어줍니다
    -argfile <arg_file>           additional command line arguments read from a file
    -blockAtHV <H V>              encodes a single block that contains the
                                  pixel specified by the H V coordinates
    -compare <comparison_image>   compares source_image to comparison_image
    -effort <amount>              인코딩 퀄리티를 의미합니다. [0-100] 
                                  (100이 가장 높은 퀄리티 입니다.)
    -errormetric <error_metric>   specify the error metric, the options are
                                  rgba, rgbx, rec709, numeric and normalxyz
    -format <etc_format>          압축포멧 입니다. ETC1, RGB8, SRGB8, RGBA8, SRGB8, RGB8A1, SRGB8A1 or R11
    -limitwidth <image width>     압축 포멧을 결정하도록하는 제한 값입니다.
    -limitheight <image height>   압축 포멧을 결정하도록하는 제한 값입니다.
    -help                         prints this message
    -jobs or -j <thread_count>    specifies the number of threads (default=1)
    -normalizexyz                 normalize RGB to have a length of 1
    -verbose or -v                shows status information during the encoding
                                  process                                  
	-mipmaps or -m <mip_count>    sets the maximum number of mipaps to generate (default=1)
	-mipwrap or -w <x|y|xy>       sets the mipmap filter wrap mode (default=clamp)

* -analyze will run an analysis of the encoding and place it in folder 
"analysis_folder" (e.g. ../analysis/kodim05).  within the analysis_folder, a folder 
will be created with a name of the current date/time (e.g. 20151204_153306).  this 
date/time folder is used to compare encodings of the same texture over time.  
within the date/time folder is a text file with several encoding stats and a 2x png 
image showing the encoding mode for each 4x4 block.

* -argfile allows additional command line arguments to be placed in a text file

* -blockAtHV selects the 4x4 pixel subset of the source image at position (H,V).  
This is mainly used for debugging

* -compare compares the source image to the created encoded image. The encoding
will dictate what error analysis is used in the comparison.

* -effort uses an "amount" between 0 and 100 to determine how much additional effort 
to apply during the encoding.

* -errormetric selects the fitting algorithm used by the encoder.  "rgba" calculates 
RMS error using RGB components that are weighted by A.  "rgbx" calculates RMS error 
using RGBA components, where A is treated as an additional data channel, instead of 
as alpha.  "rec709" is similar to "rgba", except the RGB components are also weighted 
according to Rec709.  "numeric" calculates RMS error using unweighted RGBA components.  
"normalize" calculates error based on dot product and vector length for RGB and RMS 
error for A.

* -help prints out the usage message

* -jobs enables multi-threading to speed up image encoding

* -normalizexyz normalizes the source RGB to have a length of 1.

* -verbose shows information on the current encoding process. It will then display the 
PSNR and time time it took to encode the image.

* -mipmaps takes an argument that specifies how many mipmaps to generate from the 
source image.  The mipmaps are generated with a lanczos3 filter using edge clamping.
If the mipmaps option is not specified no mipmaps are created.

* -mipwrap takes an argument that specifies the mipmap filter wrap mode.  The options 
are "x", "y" and "xy" which specify wrapping in x only, y only or x and y respectively.
The default options are clamping in both x and y.

Note: Path names can use slashes or backslashes.  The tool will convert the 
slashes to the appropriate polarity for the current platform.
